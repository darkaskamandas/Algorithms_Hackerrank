# Algorithms_Hackerrank
Algorithms Hackerrank C#, Java, C++, Python

---//Prime Digit Sums//---C#

using System;
using System.Collections;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
 
class Solution4
{
    const int NMAX = 400001;
    const int R = 1000000007;
    static bool[] PRIME = new bool[] {
        false, false, true,  true,  false, true,  false, true,  false, false,
        false, true,  false, true,  false, false, false, true,  false, true,
        false, false, false, true,  false, false, false, false, false, true,
        false, true,  false, false, false, false, false, true,  false, false,
        false, true,  false, true,  false, false, false, true,  false, true
    };
    static void Main(String[] args)
    {
        TextReader tIn = Console.In;
        TextWriter tOut = Console.Out;

        int Q = int.Parse(tIn.ReadLine());
        int[] QRY = new int[Q];
        HashSet<int> QHS = new HashSet<int>();
        for (int q = 0; q < Q; q++)
        {
            QRY[q] = int.Parse(tIn.ReadLine());
            QHS.Add(QRY[q]);
        }

        List<int[]> xy3 = new List<int[]>();
        for (int a0 = 0; a0 <= 9; a0++)
            for (int a1 = 0; a1 <= 9; a1++)
                for (int a2 = 0; a2 <= 9; a2++)
                    if (PRIME[a0 + a1 + a2])
                        xy3.Add(new int[] { a0, a1, a2 });
        int[][] x3 = xy3.ToArray();

        List<int[]> xy4 = new List<int[]>();
        foreach (int[] b3 in x3)
            for (int b = 0; b <= 9; b++)
                if (PRIME[b3[0] + b3[1] + b3[2] + b] && PRIME[b3[1] + b3[2] + b])
                    xy4.Add(new int[] { b3[0], b3[1], b3[2], b });
        int[][] x4 = xy4.ToArray();

        List<int[]> xy5 = new List<int[]>();
        foreach (int[] c4 in x4)
            for (int c = 0; c <= 9; c++)
                if (PRIME[c4[0] + c4[1] + c4[2] + c4[3] + c] && PRIME[c4[1] + c4[2] + c4[3] + c] && PRIME[c4[1] + c4[2] + c4[3]] && PRIME[c4[2] + c4[3] + c])
                    xy5.Add(new int[] { c4[0], c4[1], c4[2], c4[3], c });
        int[][] x5 = xy5.ToArray();

        int[] y5 = new int[10 * 10 * 10 * 10 * 10];
        for (int i = 0; i < 10 * 10 * 10 * 10 * 10; i++) y5[i] = -1;
        for (int i = 0; i < x5.Length; i++)
            y5[(((x5[i][0] * 10 + x5[i][1]) * 10 + x5[i][2]) * 10 + x5[i][3]) * 10 + x5[i][4]] = i;

        int[][] xx5 = new int[x5.Length][];
        int[] xx0 = new int[10];
        for (int i = 0; i < x5.Length; i++)
        {
            int nxx0 = 0;
            for (int x = 0; x <= 9; x++)
            {
                int y = y5[(((x5[i][1] * 10 + x5[i][2]) * 10 + x5[i][3]) * 10 + x5[i][4]) * 10 + x];
                if (y >= 0) xx0[nxx0++] = y;
            }
            xx5[i] = new int[nxx0];
            Array.Copy(xx0, 0, xx5[i], 0, nxx0);
        }

        long[] DPN = new long[NMAX];
        DPN[1] = 9;
        DPN[2] = 90;
        DPN[3] = x3.Where(p => p[0] > 0).Count();
        DPN[4] = x4.Where(p => p[0] > 0).Count();

        int[] DP0 = new int[x5.Length];
        int[] DP1 = new int[x5.Length];

        for (int i = 0; i < x5.Length; i++)
            if (x5[i][0] > 0) { 
                DP0[i] = 1;
                DPN[5]++;
            }

        for (int n = 6; n < NMAX; n++)
        {
            Buffer.BlockCopy(DP0, 0, DP1, 0, 4 * DP0.Length);
            Array.Clear(DP0, 0, DP0.Length);
            for (int i = 0; i < x5.Length; i++)
                if (DP1[i] > 0)
                    foreach (int xx in xx5[i])
                        DP0[xx] = (int)(((long)DP0[xx] + DP1[i]) % R);

            if (QHS.Contains(n))
                for (int i = 0; i < DP0.Length; i++)
                    DPN[n] = (DPN[n] + DP0[i]) % R;
        }

        for (int q = 0; q < Q; q++)
            tOut.WriteLine(DPN[QRY[q]]);
    }
}


---//Angry Children 2//---C++

#include <vector>
#include <list>
#include <map>
#include <set>
#include <deque>
#include <stack>
#include <algorithm>
#include <sstream>
#include <iostream>
#include <iomanip>
#include <cstdio>
#include <cmath>
#include <cstdlib>
#include <cctype>
#include <cstring>
#include <numeric>
#include <complex>
#include <string>
#include <ctime>
#include <queue>

/*
// C++ 11 stuff

#include <unordered_set>
#include <unordered_map>
#include <tuple>
#include <array>

#define tup(name, pos) get<(pos)>(name)
#define A1(type, size) array <type, size>
#define A2(type, s1, s2) A1(A1(type, s2), s1)
#define A3(type, s1, s2, s3) A1(A2(type, s2, s3), s1)

//*/


using namespace std;

typedef long long LL;
typedef unsigned long long ULL;
typedef pair <int, int> pnt;


#define FI(i,a) for (int i=0; i<(a); ++i)
#define FOR(i,s,e) for (int i=(s); i<(e); ++i)
#define MEMS(a,b) memset(a,b,sizeof(a))
#define pb push_back
#define mp make_pair
#define ALL(a) a.begin(),a.end()
#define V(t) vector < t >
#define MAX(a,b) ((a)>(b)?(a):(b))
#define MIN(a,b) ((a)<(b)?(a):(b))
#define ABS(a) ((a)>(0)?(a):(-(a)))
#define ALL(a) a.begin(),a.end()

#define dout(a) cerr << a << endl
#define sout(a) cerr << a << "  "
#define lbl cerr << "debug_label" << endl;

const double pi = 3.14159265358979323846264338327950288419716939937511;
const double eps = 1e-9;

//*
char ch_ch_ch[1<<20];
inline string gs() {scanf("%s",ch_ch_ch); return string(ch_ch_ch);}
inline string gl() {gets(ch_ch_ch); return string(ch_ch_ch);}
inline int gi() {int x; scanf("%d",&x); return x;}
//*/

const int inf = 1000000000;

// code starts here

int n;
V(int) a;
V(LL) sum;

void solution() {


    n = gi();
    int K = gi();
    FI(i,n) a.pb(gi());
    sort(ALL(a));
    sum.resize(n);
    LL res = inf*1ll*inf;
    sum[0] = a[0];
    FOR(i,1,n) sum[i] = a[i] + sum[i-1];
    LL cur = 0;
    FOR(i,1,K) cur += a[i]*1ll*i - sum[i-1];
    res = cur;
    if (K > 1) FOR(i,K,n) {
        cur += (K-1)*1ll*a[i] - (sum[i-1] - sum[i-K]);
        cur -= -(K-1)*1ll*a[i-K]  + (sum[i-1] - sum[i-K]);
        res = min(res,cur);
    }

    cout << res << endl;


}


// code ends here

int main(int argc, char** argv) {

#ifdef IGEL_ACM
    freopen("in.txt","r",stdin);
    //freopen("out.txt", "w", stdout);
    clock_t beg_time = clock();
#else
    //freopen(argv[1],"r",stdin);
    //freopen("painting.in", "r", stdin);
    //freopen("painting.out", "w", stdout);

#endif

    solution();

#ifdef IGEL_ACM
    fprintf(stderr,"*** Time: %.3lf ***\n",1.0*(clock()-beg_time)/CLOCKS_PER_SEC);
#endif

    return 0;
}


---//Sherlock's Array Merging Algorithm//---Python


#!/bin/python

import sys
from collections import Counter
MOD=10**9+7

n = int(raw_input().strip())
m = map(int, raw_input().strip().split(' '))
# your code goes here

filled={}
appendables = {(0,0):0}

lastVal=-1
for val in m:
    nextFilled={}
    nextAppendables={}
    if val>lastVal:
        for (nums,maxNums) in appendables:
            if nums+1==maxNums:
                if not (maxNums,maxNums) in nextFilled:
                    nextFilled[(maxNums,maxNums)]=0
                nextFilled[(maxNums,maxNums)]=(nextFilled[(maxNums,maxNums)]+appendables[(nums,maxNums)])%MOD
            elif maxNums==0:
                if not (nums+1,maxNums) in nextAppendables:
                    nextAppendables[(nums+1,maxNums)]=0
                nextAppendables[(nums+1,maxNums)]=(nextAppendables[(nums+1,maxNums)]+1)%MOD
            else:
                if not (nums+1,maxNums) in nextAppendables:
                    nextAppendables[(nums+1,maxNums)]=0
                nextAppendables[(nums+1,maxNums)]=(nextAppendables[(nums+1,maxNums)]+appendables[(nums,maxNums)]*(maxNums-nums))%MOD
            if nums==1:
                if not (1,nums) in nextFilled:
                    nextFilled[(1,nums)]=0
                nextFilled[(1,nums)]=(nextFilled[(1,nums)]+appendables[(nums,maxNums)]*nums)%MOD
            elif nums>1:
                if not (1,nums) in nextAppendables:
                    nextAppendables[(1,nums)]=0
                nextAppendables[(1,nums)]=(nextAppendables[(1,nums)]+appendables[(nums,maxNums)]*nums)%MOD
                
        
    else:
        for (nums,maxNums) in appendables:
            if nums==1:
                if not (1,1) in nextFilled:
                    nextFilled[(1,1)]=0
                nextFilled[(1,1)]=(nextFilled[(1,1)]+appendables[(nums,maxNums)])%MOD
            else:
                if not (1,nums) in nextAppendables:
                    nextAppendables[(1,nums)]=0
                nextAppendables[(1,nums)]=(nextAppendables[(1,nums)]+appendables[(nums,maxNums)]*nums)%MOD
    for (nums,maxNums) in filled:
        if nums==1:
            if not (1,1) in nextFilled:
                nextFilled[(1,1)]=0
            nextFilled[(1,1)]=(nextFilled[(1,1)]+filled[(nums,maxNums)])%MOD
        else:
            if not (1,nums) in nextAppendables:
                nextAppendables[(1,nums)]=0
            nextAppendables[(1,nums)]=(nextAppendables[(1,nums)]+filled[(nums,maxNums)]*nums)%MOD   
     
    lastVal=val
    appendables=nextAppendables
    filled=nextFilled
   

    
res=0


for val in filled.values():
    res=(res+val)%MOD
    
for val in appendables.values():
    res=(res+val)%MOD
    
print res
                    
