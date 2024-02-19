# JavaLayersCode-ML
Layers Code Assignment


import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class NeuralNetwork {
    // Declaring a 3D list to store weights of edges between nodes in the neural network
    private List<List<List<Double>>> weights;

    // Constructor to initialize the neural network with given number of layers and nodes in each layer
    public NeuralNetwork(int numLayers, int[] nodesInLayer) {
        weights = new ArrayList<>();

        // Loop through each layer (except the last one) to initialize weights
        for (int i = 0; i < numLayers - 1; i++) {
            List<List<Double>> layerWeights = new ArrayList<>();

            // Loop through nodes in the current layer
            for (int j = 0; j < nodesInLayer[i]; j++) {
                List<Double> nodeWeights = new ArrayList<>();

                // Initialize weights for connections from current node to nodes in the next layer
                for (int k = 0; k < nodesInLayer[i + 1]; k++) {
                    nodeWeights.add(0.0); // Initialize weight to 0.0
                }

                layerWeights.add(nodeWeights); // Add weights of current node to the layer
            }

            weights.add(layerWeights); // Add weights of current layer to the neural network
        }
    }

    // Method to set weight of an edge between two nodes in the neural network
    public void setWeight(int layer, int nodeFrom, int nodeTo, double weight) {
        weights.get(layer - 1).get(nodeFrom).set(nodeTo, weight); // Set the weight
    }

    // Method to get weight of an edge between two nodes in the neural network
    public double getWeight(int layer, int nodeFrom, int nodeTo) {
        return weights.get(layer - 1).get(nodeFrom).get(nodeTo); // Get the weight
    }

    // Main method to run the neural network
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter the number of layers: ");
        int numLayers = scanner.nextInt();
        int[] nodesInLayer = new int[numLayers];

        // Prompt user to enter number of nodes in each layer
        for (int i = 0; i < numLayers; i++) {
            System.out.print("Enter the number of nodes in layer " + i + ": ");
            nodesInLayer[i] = scanner.nextInt();
        }

        // Create a neural network with the specified number of layers and nodes in each layer
        NeuralNetwork nn = new NeuralNetwork(numLayers, nodesInLayer);
        System.out.println("Enter the weights for each edge:");

        // Prompt user to enter weights for each edge in the neural network
        for (int i = 1; i < numLayers; i++) {
            for (int j = 0; j < nodesInLayer[i - 1]; j++) {
                for (int k = 0; k < nodesInLayer[i]; k++) {
                    System.out.print("Enter the weight for edge from node " + j + " in layer " + (i - 1) + " to node " + k + " in layer " + i + ": ");
                    double weight = scanner.nextDouble();
                    nn.setWeight(i, j, k, weight); // Set the weight for the edge
                }
            }
        }

        // Prompt user to enter indices of nodes to query the weight of an edge
        System.out.println("Enter the node indices to query the weight (layer, nodeFrom, nodeTo):");
        int layer = scanner.nextInt();
        int nodeFrom = scanner.nextInt();
        int nodeTo = scanner.nextInt();

        // Get the weight of the edge between the specified nodes and layers
        double weight = nn.getWeight(layer, nodeFrom, nodeTo);
        System.out.println("Weight: " + weight); // Print the weight
    }
}
