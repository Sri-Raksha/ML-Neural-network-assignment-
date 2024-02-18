# ML-Neural-network-assignment-

import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class NeuralNetwork {
    private List<List<List<Double>>> weights;

    public NeuralNetwork(int numLayers, int[] nodesInLayer) {
        weights = new ArrayList<>();

        for (int i = 0; i < numLayers - 1; i++) {
            List<List<Double>> layerWeights = new ArrayList<>();

            for (int j = 0; j < nodesInLayer[i]; j++) {
                List<Double> nodeWeights = new ArrayList<>(nodesInLayer[i + 1]); // Initialize with capacity

                for (int k = 0; k < nodesInLayer[i + 1]; k++) {
                    nodeWeights.add(0.0);
                }

                layerWeights.add(nodeWeights);
            }

            weights.add(layerWeights);
        }
    }

    public void setWeight(int layer, int nodeFrom, int nodeTo, double weight) {
        weights.get(layer - 1).get(nodeFrom).set(nodeTo, weight);
    }

    public double getWeight(int layer, int nodeFrom, int nodeTo) {
        return weights.get(layer - 1).get(nodeFrom).get(nodeTo);
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter the number of layers: ");
        int numLayers = scanner.nextInt();
        int[] nodesInLayer = new int[numLayers];

        for (int i = 0; i < numLayers; i++) {
            System.out.print("Enter the number of nodes in layer " + i + ": ");
            nodesInLayer[i] = scanner.nextInt();
        }

        NeuralNetwork nn = new NeuralNetwork(numLayers, nodesInLayer);
        System.out.println("Enter the weights for each edge:");

        for (int i = 1; i < numLayers; i++) {
            for (int j = 0; j < nodesInLayer[i - 1]; j++) {
                for (int k = 0; k < nodesInLayer[i]; k++) {
                    System.out.print("Enter the weight for edge from node " + j + " in layer " + (i - 1) + " to node " + k + " in layer " + i + ": ");
                    double weight = scanner.nextDouble();
                    nn.setWeight(i, j, k, weight);
                }
            }
        }

        System.out.println("Enter the node indices to query the weight (layer, nodeFrom, nodeTo):");
        int layer = scanner.nextInt();
        int nodeFrom = scanner.nextInt();
        int nodeTo = scanner.nextInt();

        double weight = nn.getWeight(layer, nodeFrom, nodeTo);
        System.out.println("Weight: " + weight);
    }
}
