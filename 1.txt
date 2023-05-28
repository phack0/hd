// design and implement parallel breath first search and depth first search based on existing algorithm using openMP use Tree or an undirected graph for bfs and dfs
#include <bits/stdc++.h>
#include <omp.h>
#include <chrono>
using namespace std::chrono;
using namespace std;

int N;
vector<int> graph [100000];
void bfs(int start) {
	vector<bool> vis(N);
	queue<int> q;
	q.push(start);

	while(!q.empty()) {
		int cur = q.front();
		q.pop();
		if(!vis[cur]) {
			vis[cur] = 1; cout << cur <<" ";
			
			#pragma omp parallel for
			for (int next: graph[cur]) {
				if(!vis[next]) q.push(next);			
			}		
		}	
	}
}

void sbfs(int start) {
	vector<bool> vis(N);
	queue<int> q;
	q.push(start);

	while(!q.empty()) {
		int cur = q.front();
		q.pop();
		if(!vis[cur]) {
			vis[cur] = 1; cout << cur <<" ";
			for (int next: graph[cur]) {
				if(!vis[next]) q.push(next);			
			}		
		}	
	}
}

int main(){
	int n;cin>>N>>n;
	for(int i=0;i<n;i++){
		int a,b;
		cin>>a>>b;
		graph[a].push_back(b);
		graph[b].push_back(a);
	}
	int startn;cin>>startn;
	auto start = high_resolution_clock::now();
	sbfs(startn);
	auto stop = high_resolution_clock::now();
   	auto duration = duration_cast<microseconds>(stop - start);
	cout<<" time taken by seq bfs: "<<duration.count()<<endl;
	start = high_resolution_clock::now();
	bfs(startn);
	stop = high_resolution_clock::now();
   	duration = duration_cast<microseconds>(stop - start);
	cout<<" time taken by parallel bfs: "<<duration.count()<<endl;	
	return 0;
}
/*
9 10
1 2
2 3
2 4
3 5
5 7
6 1
8 3
3 5
6 8
9 1
1
*/