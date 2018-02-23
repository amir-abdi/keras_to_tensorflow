# keras_to_tensorflow
General code to convert a trained keras model into an inference tensorflow model

The notebook ```keras_to_tensorflow```, is a sample code which loads a trained keras model, freezes the nodes (converts all tensorflow variables to tensorflow constants), and saves the inference graph and weights into a protobuf file (.pb). This file can then be used to deploy the trained model for inference. During freezing, other nodes of the network, which do not contribute  the tensor that would contain the output predictions, are prunned. This results in a smaller, optimized network.

The code on how to freeze and save keras models in previous versions of tensorflow is also available. Back then, the freeze_graph tool (```/tensorflow/python/tools/freeze_graph.py```) was used to convert the variables into constants. This functionality is now handled by ```graph_util.convert_variables_to_constants```

## How to use
The keras model can be saved using `model.save('file_name.h5')` (check keras API documentation for details).

You can either use the iPython notebook (`kears_to_tensorflow.ipnyb`), or simply run the python script as below in the folder where your keras model is present:

    python3 keras_to_tensorflow.py -input_model_file model.h5
    python keras_to_tensorflow.py -input_model_file model.h5 

Try `python3 keras_to_tensorflow.py --help` for other input arguments.

## Input arguments
- num_output: this value has nothing to do with the number of classes, batch_size, etc., and it is mostly equal to 1. If the network is a **multi-stream network** (forked network with multiple outputs), set the value to the number of outputs. 

- quantize: if set to True, use the quantize feature of Tensorflow (https://www.tensorflow.org/performance/quantization) [default: False]

- use_theano: Thaeno and Tensorflow implement convolution in different ways. When using Keras with Theano backend, the order is set to 'channels_first'. This feature is not fully tested, and doesn't work with quantizization [default: False]

- input_fld: directory holding the keras weights file [default: .]

- output_fld: destination directory to save the tensorflow files [default: .]

- input_model_file: name of the input weight file [default: 'model.h5']

- output_model_file: name of the output weight file [default: args.input_model_file + '.pb']

- graph_def: if set to True, will write the graph definition as an ascii file [default: False]

- output_graphdef_file: if graph_def is set to True, the file name of the graph definition [default: model.ascii]

- output_node_prefix: the prefix to use for output nodes. [default: output_node]

## Not tested features:
Theano support is not yet fully tested. I don't assume loading a theano model in Keras and saving it as a Tensorflow model would work without any proper conversion between the two. The feature is committed for theano enthusiasts to test.

## Dependencies
- Keras
- Tensorflow
- argparse
- pathlib
