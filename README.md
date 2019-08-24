# interview
面试要点整理

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <algorithm>
#include <functional>
#include <array>
#include <cmath>
#include <cstdio>
#include <map>
//c
//#include <math.h>
//#include <stdio.h>
using namespace std;

bool cmp(pair<int, int> a, pair<int, int> b) {
    return a.second < b.second;
}

int main() {
    unsigned int C=0;
    cin>>C;
    int num,role;
    vector<int>roles;
    vector<vector<int>>nums(C,vector<int>());
    vector<int>res;
    for (int i = 0; i < C; i++) {
        cin>>role;
        roles.push_back(role);
        for (int j = 0; j < role; j++) {
            cin>>num;
            nums[i].push_back(num);
        }
    }

    for(int i=0;i<C;i++){
        role=roles[i];
        sort(nums[i].begin(),nums[i].end());
        int j=0;
        while(nums[i][j]<=0){
            j++;
        }
        if(role-j<3){
            res.push_back(0);
            continue;
        }else if(role-j==3){
            res.push_back(nums[i][j]);
            continue;
        }
        int tmp=0;
        while(nums[i][role-3]!=0){
            tmp+=nums[i][role-3];
            sort(nums[i].begin(),nums[i].end());
        }
        res.push_back(nums[i][j]*(role-j-1)+nums[i][j+1]);

    }
    for(int i=0;i<C;i++){
        cout<<res[i]<<endl;
    }
    return 0;
}

```