class Solution {
public:
    int minCostConnectPoints(vector<vector<int>>& points) {
        // creating adj list
        int n = points.size();              // total number of vertices
        vector<pair<int,int>> adj[n];

        for(int i = 0 ; i < n; i++){
            int x1 = points[i][0];
            int y1 = points[i][1];
            for(int j = i+1; j < n; j++){
                int x2 = points[j][0];
                int y2= points[j][1];

                int wt = abs(x1-x2) + abs(y1-y2);

                adj[i].push_back({j,wt});
                adj[j].push_back({i,wt});
            }
        }

        // prims algorithms to find the cost of MST
        vector<bool> vis(n,false);
        int mst = 0;
        priority_queue<pair<int,int>> pq;     // wt, node

        pq.push({0,0});

        while(!pq.empty()){
            pair<int,int> frontNode = pq.top();
            int wt = -1 * frontNode.first;
            int node = frontNode.second;
            pq.pop();

            if(vis[node]) continue;
            vis[node] = true;
            mst += wt;
            for(auto nbr : adj[node]){
                int nbrVal = nbr.first;
                int nbrWt = -1 * nbr.second;

                if(!vis[nbrVal]){
                    pq.push({nbrWt,nbrVal});
                }
            }
        }
        return mst;

    }
};