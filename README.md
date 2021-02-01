# pytorch2timeloop-converter
Converting pytorch nn.Conv2d modules, which describe 2D convolutional layers in neural networks, to timeloop workload yaml files. 

### Installing the converter
After cloning this repository, run `python setup.py install` to finish the installation. Note that this converter has been developed and tested with: 

- python 3.7
- pytorch 1.7.1
- torchvision 0.8.2
- numpy 1.19.2
- pyyaml 5.3.1

### Using the converter
```python
import torchvision.models as models
import converter

# Define a pytorch-based neural network model, for example, a pre-defined alexnet from torchvision.
net = models.alexnet()

# Define the shape of a single input sample, in the following format:
# (# of channels, height, width)
# For example, the above alexnet will get a 224x224 RGB image:
input_shape = (3, 224, 224)

# Define the number of batches that will be used for the inference
batch_size = 1

# Define the directory names where the timeloop workload yaml files will be stored.
# The yaml files will be stored in ./workloads/alexnet/ in this example.
top_dir = 'workloads'
sub_dir = 'alexnet'

# Finally, in case there exists a layer that is only used during the training phase, define an identifier for a such layer. 
# For example, in torchvision.models.inception_v3, auxiliary classification layers are not used during the inference (e.g., InceptionAux).
# In this case, include a string that can serve as an identifier for such layers (e.g., 'Aux') in exception_module_names.
# But for the above alexnet, there is no necessity to define this. 
exception_module_names = []

# Now, convert!
converter.model_converter_pytorch(net, input_shape, batch_size, sub_dir, top_dir, exception_module_names)
```

---

This code is licensed with MIT License. This code has been modified from the works of Anurag Golla and Alex Moser.


