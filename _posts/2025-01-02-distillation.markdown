---
layout: post
title:  "Distillation"
date:   2025-01-02 00:00:00 -0000
categories: machinelearning
---

In this page, I will talk about few tips related to distillation in neural networks. Main reference for this page is the original [distillation paper](https://arxiv.org/pdf/1503.02531).

# Distillation

* When the soft targets have high entropy, they provide much more information per training case than hard targets and much less variance in the gradient between training cases, so the small model can often be trained on much
less data than the original cumbersome model and using a much higher learning rate.

* In distillation, the temperature of the final softmax is raised until the cumbersome model produces a suitably soft set of targets. We then use the same high temperature when training the small model to match these
soft targets.

* Let the logits for the student model prior to softmax be $z_i$. These logits are converted to probabilities $q_i$ using a temperature $T$ according to the following equation in which $T$ is a temperature that is normally set to 1. Using higher values for $T$ produces a softer probability distribution over classes with higher entropy.

$$
q_i = \frac{\exp(z_i/T)}{\sum_j \exp(z_j/T)}
$$

* In the simplest form of distillation, knowledge is transferred to the distilled model by training it on
a transfer set and using a soft target distribution for each case in the transfer set that is produced by
using the cumbersome model with a high temperature in its softmax. The same high temperature is
used when training the distilled model, but after it has been trained it uses a temperature of 1.

* When the correct labels are known for all or some of the transfer set, this method can be significantly
improved by also training the distilled model to produce the correct labels. One way to do this is
to use the correct labels to modify the soft targets, but a better way is to simply use
a weighted average of two different objective functions. 

* The first objective function is the cross entropy with the soft targets and this cross entropy is computed using the same high temperature in the softmax of the distilled model as was used for generating the soft targets from the cumbersome model. If the cumbersome model produces soft-target probabilies $p_i$, then the first objective function can be written as

$$
C_1 = - \sum_{i=1}^{N} p_i \log(q_i) 
$$

* The second objective function is the cross entropy with the correct labels. This is computed
using exactly the same logits in softmax of the distilled model but at a temperature of 1.

* The best results are generally obtained by using a condiderably lower weight on the second
objective function. Since the magnitudes of the gradients produced by the soft targets scale as $1/{T^2}$
it is important to multiply them by $T^2$ when using both hard and soft targets. This ensures that the
relative contributions of the hard and soft targets remain roughly unchanged if the temperature used
for distillation is changed while experimenting with meta-parameters.

* If the cumbersome model has logits $v_i$, then for high temperatures and for the case  that the logits have been zero-meaned separately for each transfer case ($\sum_j z_j = \sum_j v_j = 0$) we can show that 

$$
\frac{\partial C_1}{\partial z_i} = \frac{1}{T}(q_i - p_i) = \frac{1}{N T^2}(z_i - v_i) 
$$

* So in the high temperature limit, distillation is equivalent to minimizing $1/2(z_i − v_i)^2$, provided the
logits are zero-meaned separately for each transfer case. At lower temperatures, distillation pays much less attention to matching logits that are much more negative than the average. This is potentially advantageous because these logits are almost completely unconstrained by the cost function
used for training the cumbersome model so they could be very noisy. 

* On the other hand, the very negative logits may convey useful information about the knowledge acquired by the cumbersome
model. Which of these effects dominates is an empirical question. When the distilled
model is much too small to capture all of the knowledege in the cumbersome model, intermediate temperatures work best which strongly suggests that ignoring the large negative logits can be
helpful.

* In the following code we tried to replicate the experiment on MNIST data in the distillation paper and we trained a teacher model in standalone mode for 300 epochs and a student model in standalone mode for 200 epochs and a distillation model for 600 epochs. Standalone precision for teacher model is 99.01% and for student model is 98.81% and for the distillation model is 98.91%. Notice that our results differ from the ones in the distillation paper because we trained the small model in standalone mode using the jittered inputs and that seems to have increase the precision of the small model. Also we used jittered model for training the distillation models which was different from the paper. However, we noticed that we got improved results when training using distillation compared to standalone. 

{% highlight python %}
import os
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import torch
import torch.nn as nn
import torch.nn.functional as F
import torchvision
from torchvision import datasets, transforms
from torch.utils.data import DataLoader

# Large model is using dropout. Dropout in effect is like a geometric ensemble of many smaller models. 
# Therefore large model is a big ensemble model. Highly regularized and well-trained. 
LARGE_HIDDEN_SIZE = 1200
class LargeNet(nn.Module):
    def __init__(self) -> None:
        super().__init__()
        self.dense1 = nn.Linear(28*28, LARGE_HIDDEN_SIZE)
        self.dropout1 = nn.Dropout(p=0.5) 
        self.dense2 = nn.Linear(LARGE_HIDDEN_SIZE, LARGE_HIDDEN_SIZE)
        self.dropout2 = nn.Dropout(p=0.5) 
        self.dense3 = nn.Linear(LARGE_HIDDEN_SIZE, 10)

    def forward(self, x):
        x = F.relu(self.dense1(x))
        x = self.dropout1(x)
        x = F.relu(self.dense2(x))
        x = self.dropout2(x)
        x = self.dense3(x)
        return x

# Small model is not using dropout. It is just a single light and small mode. 
SMALL_HIDDEN_SIZE = 800
class SmallNet(nn.Module):
    def __init__(self) -> None:
        super().__init__()
        self.dense1 = nn.Linear(28*28, SMALL_HIDDEN_SIZE) 
        self.dense2 = nn.Linear(SMALL_HIDDEN_SIZE, SMALL_HIDDEN_SIZE)
        self.dense3 = nn.Linear(SMALL_HIDDEN_SIZE, 10)

    def forward(self, x):
        x = F.relu(self.dense1(x))
        x = F.relu(self.dense2(x))
        x = self.dense3(x)
        return x

# Training a standalone model
def train_standalone(model, dataloader, optimizer, criterion):
    model.train()
    train_loss = 0.0
    for inputs, labels in dataloader:
      inputs, labels = inputs.to(device), labels.to(device) 
      inputs = inputs.view(inputs.size(0), -1)
      optimizer.zero_grad()
      logits = model(inputs)
      loss = criterion(logits, labels)
      loss.backward()
      optimizer.step()
      train_loss += loss.item() * inputs.size(0)
    train_loss /= len(dataloader.dataset)
    return train_loss

def compute_val_loss(model, val_inputs, val_labels, criterion):
  model.eval()  # Set model to evaluation mode
  val_loss = 0.0
  with torch.no_grad():  # No gradient computation
    val_outputs = model(val_inputs)
    val_loss = criterion(val_outputs, val_labels)
  return val_loss

def compute_score(model, val_inputs, val_labels):
    model.eval()
    with torch.no_grad():
        outputs = model(val_inputs)
        probs, idx = outputs.max(dim=1)
    correctly_found = (idx == val_labels)
    precision = sum(correctly_found)/correctly_found.size(0)
    return precision, idx, probs, val_labels

def save_checkpoint(model, optimizer, epoch, filepath):
    checkpoint = {
        'model_state_dict': model.state_dict(),
        'optimizer_state_dict': optimizer.state_dict(),
        'epoch': epoch
    }
    torch.save(checkpoint, filepath)
    print(f"Checkpoint saved at {filepath}")

# Knowledge distillation loss
def distillation_loss(student_logits, teacher_logits, temperature):
    teacher_probs = F.softmax(teacher_logits / temperature, dim=1)
    student_log_probs = F.log_softmax(student_logits / temperature, dim=1)
    return F.kl_div(student_log_probs, teacher_probs, reduction='batchmean') * (temperature ** 2)

def train_student(student, teacher, dataloader, optimizer, criterion, temperature, alpha):
    teacher.eval()  # Teacher is frozen during student training
    student.train()
    distill_loss = 0.0
    for inputs, labels in dataloader:
        inputs, labels = inputs.pin_memory().to(device, non_blocking=True), labels.pin_memory().to(device, non_blocking=True)
        # inputs, labels = inputs.to(device), labels.to(device)
        inputs = inputs.view(inputs.size(0), -1)
        optimizer.zero_grad()

        teacher_logits = teacher(inputs).detach()
        student_logits = student(inputs)

        hard_loss = criterion(student_logits, labels)
        soft_loss = distillation_loss(student_logits, teacher_logits, temperature)
        loss = alpha * soft_loss + (1 - alpha) * hard_loss

        loss.backward()
        optimizer.step()
        distill_loss += loss.item() * inputs.size(0)
    distill_loss /= len(dataloader.dataset)
    return distill_loss

# Transform to normalize data
transform = transforms.Compose([
    transforms.ToTensor(),
    transforms.RandomAffine(degrees=0, translate=(2/28, 2/28)),
    transforms.Normalize((0.5,), (0.5,))
    ])

# Download and load the dataset
train_dataset = datasets.MNIST(root='./data', train=True, download=True, transform=transform)
test_dataset = datasets.MNIST(root='./data', train=False, download=True, transform=transform)

TRAIN_BATCH_SIZE = 8192
TEST_BATCH_SIZE = len(test_dataset) # we can load the entire test dataset in one batch for more efficient computations

train_loader = DataLoader(train_dataset, batch_size=TRAIN_BATCH_SIZE, shuffle=True, num_workers=os.cpu_count(), pin_memory=True)
test_loader = DataLoader(test_dataset, batch_size=TEST_BATCH_SIZE, shuffle=False, num_workers=os.cpu_count(), pin_memory=True)

train_losses_teacher = []
val_losses_teacher = []

train_losses_student = []
val_losses_student = []

distillation_train_losses = []
distillation_val_losses = []

teacher_num_epochs = 300
student_num_epochs = 200
distill_num_epochs = 600
temperature = 20
alpha = 0.7
teacher_learning_rate = 0.001
student_learning_rate = 0.001
distill_learning_rate = 0.001

device = 'cuda' if torch.cuda.is_available() else 'cpu'
print(f'Using {device} device')

criterion = nn.CrossEntropyLoss()

test_inputs, test_labels = next(iter(test_loader))
test_inputs = test_inputs.view(test_inputs.size(0), -1)
test_inputs = test_inputs.to(device)
test_labels = test_labels.to(device)
print(test_inputs.size(), test_labels.size())

teacher = LargeNet().to(device)
optimizer_teacher = torch.optim.Adam(teacher.parameters(), lr=teacher_learning_rate)

print("Training the teacher model", "\n")
for epoch in range(teacher_num_epochs):
  train_loss_teacher = train_standalone(teacher, train_loader, optimizer_teacher, criterion)
  eval_loss_teacher = compute_val_loss(teacher, test_inputs, test_labels, criterion)
  print(f"Epoch [{epoch+1}/{teacher_num_epochs}], "
        f"Train Loss: {train_loss_teacher:.4f}, "
        f"Val Loss: {eval_loss_teacher:.4f}")
  train_losses_teacher.append(train_loss_teacher)
  val_losses_teacher.append(eval_loss_teacher)

p_teacher, teacher_preds, teacher_probs, teacher_labels = compute_score(teacher, test_inputs, test_labels)
print("Teacher precision: ", round(100*p_teacher.cpu().numpy(), 2), "%")

teacher_path = "/kaggle/working/checkpoints/distillation/teacher_p5_p5_64_300.pth"
save_checkpoint(teacher, optimizer_teacher, teacher_num_epochs, teacher_path)

student = SmallNet().to(device)
optimizer_student_standalone = torch.optim.Adam(student.parameters(), lr=student_learning_rate)

print("\n","Training the student model standalone...", "\n")
for epoch in range(student_num_epochs):
  train_loss_student = train_standalone(student, train_loader, optimizer_student_standalone, criterion)
  eval_loss_student = compute_val_loss(student, test_inputs, test_labels, criterion)
  print(f"Epoch [{epoch+1}/{student_num_epochs}], "
        f"Train Loss: {train_loss_student:.4f}, "
        f"Val Loss: {eval_loss_student:.4f}")
  train_losses_student.append(train_loss_student)
  val_losses_student.append(eval_loss_student)

p_student, student_preds, student_probs, student_labels = compute_score(student, test_inputs, test_labels)
print("Standalone student precision: ", round(100*p_student.cpu().numpy(), 2), "%")

student_path = "/kaggle/working/checkpoints/distillation/student_standalone_8192_200.pth"
save_checkpoint(student, optimizer_student_standalone, student_num_epochs, student_path)

teacher_model = LargeNet().to(device)
teacher_path = "/kaggle/working/checkpoints/distillation/teacher_p5_p5_8192_300.pth"
teacher_model_checkpoint = torch.load(teacher_path, weights_only=False)
teacher_model.load_state_dict(teacher_model_checkpoint['model_state_dict'])

distilled_model = SmallNet().to(device)
distillation_optimizer = torch.optim.Adam(distilled_model.parameters(), lr=distill_learning_rate)

print("Training student network through distillation...")
for epoch in range(distill_num_epochs):  # Adjust epochs as needed
    distillation_trainining_loss = train_student(distilled_model,
                                                 teacher_model,
                                                 train_loader,
                                                 distillation_optimizer,
                                                 criterion,
                                                 temperature,
                                                 alpha)
    distillation_eval_loss = compute_val_loss(distilled_model, test_inputs, test_labels, criterion)
    print(f"Epoch [{epoch+1}/{distill_num_epochs}], "
        f"Distillation Training Loss: {distillation_trainining_loss:.4f}, "
        f"Distillation Val Loss: {distillation_eval_loss:.4f}")
    distillation_train_losses.append(distillation_trainining_loss)
    distillation_val_losses.append(distillation_eval_loss)

distillation_path = "/kaggle/working/checkpoints/distillation/distillation_8192_600.pth"
save_checkpoint(distilled_model, distillation_optimizer, 600, distillation_path)

p_distill, distill_preds, distill_probs, distill_labels = compute_score(distilled_model, test_inputs, test_labels)
print("Distillation model (600 epochs) precision: ", round(100*p_distill.cpu().numpy(), 2), "%")

teacher_model = LargeNet().to(device)
print("Loading trained teacher model...")
teacher_path = "/kaggle/working/checkpoints/distillation/teacher_p5_p5_8192_300.pth"
teacher_model_checkpoint = torch.load(teacher_path, weights_only=False, map_location=torch.device('cpu'))
teacher_model.load_state_dict(teacher_model_checkpoint['model_state_dict'])
teacher_model.eval()
test_outputs = teacher_model(test_inputs)
_, test_idx = test_outputs.max(dim=1)
equalit = test_idx == test_labels
print(sum(equalit.detach().cpu().numpy()))

student_model = SmallNet().to(device)
print("Loading standalone trained student model...")
student_path = "/kaggle/working/checkpoints/distillation/student_standalone_8192_200.pth"
student_model_checkpoint = torch.load(student_path, weights_only=False, map_location=torch.device('cpu'))
student_model.load_state_dict(student_model_checkpoint['model_state_dict'])
student_model.eval()
test_outputs = student_model(test_inputs)
_, test_idx = test_outputs.max(dim=1)
equalit = test_idx == test_labels
print(sum(equalit.detach().cpu().numpy()))

distillation_path = "/kaggle/working/checkpoints/distillation/distillation_8192_600.pth"
print("Loading trained distilled model...")
distilled_model = SmallNet().to(device)
distilled_model_checkpoint = torch.load(distillation_path, weights_only=False, map_location=torch.device('cpu'))
distilled_model.load_state_dict(distilled_model_checkpoint['model_state_dict'])
distilled_model.eval()
test_outputs = distilled_model(test_inputs)
_, test_idx = test_outputs.max(dim=1)
equalit = test_idx == test_labels
print(sum(equalit.detach().cpu().numpy()))
{% endhighlight %}
