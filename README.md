# february15_2025
The problems that I solved today

1.Given a positive integer n, return the punishment number of n. The punishment number of n is defined as the sum of the squares of all integers i such that: 1 <= i <= n The decimal representation of i * i can be partitioned into contiguous substrings such that the sum of the integer values of these substrings equals i.

Code:
class Solution {
    public boolean f(int ind,int sum,String s,int target,int[][] dp)
    {
        if(ind==s.length()) 
            return sum==target;
        if(sum>target)
            return false;
        if(dp[ind][sum]!=-1)
            return dp[ind][sum]==1;
        boolean flag=false;
        for(int i=ind;i<s.length();i++)
        {
            String x=s.substring(ind,i+1);
            int num=Integer.valueOf(x);
            flag=flag || f(i+1,sum+num,s,target,dp);
            if(flag)
            {
                dp[ind][sum]=1;
                return true;
            }
        }
        dp[ind][sum]=0;
        return false;
    }
    public int punishmentNumber(int n) {
        int i,res=0;
        for(i=1;i<=n;i++)
        {
            String s=Integer.toString(i*i);
            int[][] dp=new int[s.length()][i+1];
            for(int[] x:dp)
                Arrays.fill(x,-1);
            if(f(0,0,s,i,dp))
                res+=i*i;
        }
        return res;
    }
}
 
2.There is a dungeon with n x m rooms arranged as a grid. You are given a 2D array moveTime of size n x m, where moveTime[i][j] represents the minimum time in seconds when you can start moving to that room. You start from the room (0, 0) at time t = 0 and can move to an adjacent room. Moving between adjacent rooms takes one second for one move and two seconds for the next, alternating between the two. Return the minimum time to reach the room (n - 1, m - 1). Two rooms are adjacent if they share a common wall, either horizontally or vertically.

Code:
class Solution {
    public int minTimeToReach(int[][] moveTime) {
        int n=moveTime.length,m=moveTime[0].length,i,j;
        PriorityQueue<int[]> pq=new PriorityQueue<>((a,b)->Integer.compare(a[0],b[0]));
        int[][] dist=new int[n][m];
        for(int[] x:dist)
            Arrays.fill(x,Integer.MAX_VALUE);
        int[][] dir={{0,-1},{-1,0},{0,1},{1,0}};
        dist[0][0]=0;
        pq.add(new int[]{0,0,0});
        int t=1;
        while(!pq.isEmpty())
        {
            int[] x=pq.poll();
            int dis=x[0];
            int r=x[1];
            int c=x[2];
            if(r==n-1 && c==m-1)
                return dis;
            for(int[] d:dir)
            {
                int nr=r+d[0];
                int nc=c+d[1];
                if(nr>=0 && nr<n && nc>=0 && nc<m)
                {
                    int val=Math.max(dis,moveTime[nr][nc])+t;
                    if(val<dist[nr][nc])
                    {
                        dist[nr][nc]=val;
                        t=t==1?2:1;
                        pq.add(new int[]{dist[nr][nc],nr,nc});
                    }
                }
            }
            System.out.println(r+" "+c+" "+dist[r][c]);
        }
        return -1;
    }
}
