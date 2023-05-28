#include <bits/stdc++.h>
#include <omp.h>
using namespace std;

int arr[]={2 ,1 ,5 ,7 ,6 ,3};
int n=6;

void max_reduction(){
	int maximum=arr[0];
	#pragma omp parallel for reduction(max:maximum)
	for(int i=0;i<n;i++){
		if(arr[i]>maximum){
			maximum=arr[i];		
		}	
	}
	cout<<"maximum element is: "<<maximum<<endl;
}

void min_reduction(){
	int minimum=arr[0];
	#pragma omp parallel for reduction(min:minimum)
	for(int i=0;i<n;i++){
		if(arr[i]<minimum){
			minimum=arr[i];		
		}	
	}
	cout<<"minimum element is: "<<minimum<<endl;
}

void sum_reduction(){
	int total=0;
	#pragma omp parallel for reduction(+:total)
	for(int i=0;i<n;i++){
		total=total+arr[i]
	}
	cout<<"sum of element is: "<<total<<endl;
}

void avg_reduction(){
	int total=0;
	#pragma omp parallel for reduction(+:total)
	for(int i=0;i<n;i++){
		total=total+arr[i]
	}
	cout<<"avg of element is: "<<(total/(double)n)<<endl;
}

int main(){
	min_reduction();
	max_reduction();
	sum_reduction();
	avg_reduction();
	return 0;
}