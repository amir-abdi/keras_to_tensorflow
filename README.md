# keras_to_tensorflow
General code to convert a trained keras model into an inference tensorflow model

The notebook ```keras_to_tensorflow```, is a sample code which loads a trained keras model, freezes the nodes (converts all tensorflow variables to tensorflow constants), and saves the inference graph and weights into a protobuf file (.pb). This file can then be used to deploy the trained model for inference. During freezing, other nodes of the network, which do not contribute  the tensor that would contain the output predictions, are prunned. This results in a smaller, optimized network.

The code on how to freeze and save keras models in previous versions of tensorflow is also available. Back then, the freeze_graph tool (```/tensorflow/python/tools/freeze_graph.py```) was used to convert the variables into constants. This functionality is now handled by ```graph_util.convert_variables_to_constants```

## How to use
The keras model can be saved using `model.save('file_name.h5')` (check keras API documentation for details).

You can either use the iPython notebook (`kears_to_tensorflow.ipnyb`), or simply run the python script as below in the folder where your keras model is present:

    python3 keras_to_tensorflow.py -input_model_file model.h5 -output_model_file model.pb

Try `python3 keras_to_tensorflow.py --help` for other input arguments.

## Dependencies
- Keras
- Tensorflow
- argparse
