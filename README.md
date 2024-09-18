import org.json.JSONArray;
import org.json.JSONObject;

import java.util.ArrayList;
import java.util.List;

public class ShamirSecretSharing {

  public static void main(String[] args) {
        // Sample JSON input
        String jsonInput = "{\"n\": 3, \"k\": 3, \"roots\": [{\"x\": 2, \"y\": \"111\"}, {\"x\": 3, \"y\": \"1010\"}, {\"x\": 4, \"y\": \"10001\"}]}";
        
       // Parse the JSON input
        JSONObject jsonObject = new JSONObject(jsonInput);
        int n = jsonObject.getInt("n");
        int k = jsonObject.getInt("k");
        JSONArray roots = jsonObject.getJSONArray("roots");

        // Decode y values and store (x, y) pairs
        List<Point> points = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            JSONObject root = roots.getJSONObject(i);
            int x = root.getInt("x");
            int y = decodeY(root.getString("y"));
            points.add(new Point(x, y));
        }

        // Calculate the constant term c using Lagrange interpolation
        double c = lagrangeInterpolation(points, 0);
        
        // Output the secret c
        System.out.println("The secret c is: " + c);
    }

    private static int decodeY(String y) {
        return Integer.parseInt(y, 2); // Decode from binary to integer
    }

    private static double lagrangeInterpolation(List<Point> points, double x) {
        double result = 0.0;
        for (int i = 0; i < points.size(); i++) {
            double term = points.get(i).y;
            for (int j = 0; j < points.size(); j++) {
                if (i != j) {
                    term *= (x - points.get(j).x) / (points.get(i).x - points.get(j).x);
                }
            }
            result += term;
        }
        return result;
    }

    static class Point {
        int x;
        double y;

        Point(int x, double y) {
            this.x = x;
            this.y = y;
        }
    }
}
