#include <bits/stdc++.h>
#include <omp.h>

using namespace std;

// Function to generate a random dataset
void generate_dataset(vector<double>& x, vector<double>& y, int n) {
    srand(time(nullptr));
    for (int i = 0; i < n; i++) {
        double xi = rand() / (double)RAND_MAX;
        double yi = 2 * xi + 1 + rand() / (double)RAND_MAX;
        x.push_back(xi);
        y.push_back(yi);
    }
}

// Function to perform normal linear regression
void normal_linear_regression(vector<double>& x, vector<double>& y, double& slope, double& intercept) {
    double sum_x = 0, sum_y = 0, sum_xy = 0, sum_x2 = 0;
    int n = x.size();
    for (int i = 0; i < n; i++) {
        sum_x += x[i];
        sum_y += y[i];
        sum_xy += x[i] * y[i];
        sum_x2 += x[i] * x[i];
    }
    slope = (n * sum_xy - sum_x * sum_y) / (n * sum_x2 - sum_x * sum_x);
    intercept = (sum_y - slope * sum_x) / n;
}

// Function to perform parallel linear regression using OpenMP
void parallel_linear_regression(vector<double>& x, vector<double>& y, double& slope, double& intercept) {
    double sum_x = 0, sum_y = 0, sum_xy = 0, sum_x2 = 0;
    int n = x.size();
    #pragma omp parallel for reduction(+:sum_x,sum_y,sum_xy,sum_x2)
    for (int i = 0; i < n; i++) {
        sum_x += x[i];
        sum_y += y[i];
        sum_xy += x[i] * y[i];
        sum_x2 += x[i] * x[i];
    }
    slope = (n * sum_xy - sum_x * sum_y) / (n * sum_x2 - sum_x * sum_x);
    intercept = (sum_y - slope * sum_x) / n;
}

int main() {
    // Generate a random dataset
    vector<double> x, y;
    int n = 10000000;
    generate_dataset(x, y, n);

    // Perform normal linear regression and measure the time it takes
    double slope, intercept;
    auto start = chrono::high_resolution_clock::now();
    normal_linear_regression(x, y, slope, intercept);
    auto stop = chrono::high_resolution_clock::now();
    auto duration = chrono::duration_cast<chrono::microseconds>(stop - start);
    cout << "Normal linear regression took " << duration.count() << " microseconds." << endl;

    // Perform parallel linear regression using OpenMP and measure the time it takes
    start = chrono::high_resolution_clock::now();
    parallel_linear_regression(x, y, slope, intercept);
    stop = chrono::high_resolution_clock::now();
    duration = chrono::duration_cast<chrono::microseconds>(stop - start);
    cout << "Parallel linear regression using OpenMP took " << duration.count() << " microseconds." << endl;

    return 0;
}