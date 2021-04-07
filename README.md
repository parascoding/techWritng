# First 
Find the maximum of minimum for every window size in a given array - 

We have an array of size ‘n’, We need to find the maximum of minimums of every window size in the array. Where the window size ranges from 1 to n.
Example - 
Input - arr[ ] = {15, 20, 8, 30, 12, 20}
Output - 30, 15, 12, 8, 8, 8

Explanation - First element (30) of output is the maximum of minimums of all windows of size 1.
Windows of size 1 are - {15}, {20}, {8}, {30}, {12}, {20} out of which maximum is 30. 

The second element of output represents the maximum of minimums of all windows of size 2.
Windows of size 2 are - {15,20}, {20,8}, {8,30}, {30,12}, {12,20} Their minimum are 15, 8, 8, 12, 12 respectively and their maximum is 15. 

The third element of output represents the maximum of minimums of all windows of size 3. Minimums of windows of size 3 are {8}, {8}, {8}, and {12} and their maximum is 12.

Similarly, other elements of the output are calculated. 




Simple approach (Brute Force) - 
The brute force approach is to generate and check for all windows of size ‘x’ where x ranges from 1 to n. Then to find the minimum element of all windows for a particular value of x and storing the maximum of those minimums. 

To generate all windows we need to know about starting and end points of windows which can be done easily by two nested loops. And for checking for windows of all sizes from 1 to n. We need another nested loop.

Ultimately we have to use 3 nested loops for window-size, starting point and end point. 

The implementation of the approach discussed above is as follows:

CPP:

#include <bits/stdc++.h>
using namespace std;
void printMaxofMinForEveryWindow(int arr[],int n){
    // Checking for windows of all valid sizes i.e. from 1 to n
    for(int windowSize=1;windowSize<=n;windowSize++)
    {
        // Initializing maximum with minimum possible value
        int maximum=INT_MIN;
        
        // Traversing through all windows of current windowSize
        for(int i=0;i<=n-windowSize;i++)
        {
            // Checking for minimum element in current window
            int minimum=arr[i];
            for(int j=1;j<windowSize;j++)
            {
                minimum=min(minimum,arr[i+j]);
            }
            // Updating value of max if the minimum of this window is greater than max. 
            maximum=max(maximum,minimum);
        }
        // Print max of min for all window of current windowSize
        cout<<maximum<<" ";
    }
}
int main() {
	int arr[]={15, 20, 8, 30, 12, 20};
	int n = sizeof(arr)/sizeof(arr[0]);
	printMaxofMinForEveryWindow(arr,n);
	return 0;
}


JAVA:

import java.util.*;
public class Main {
    public static void printMaxofMinForEveryWindow(int arr[],int n){
        // Checking for windows of all valid sizes i.e. from 1 to n
        for(int windowSize=1;windowSize<=n;windowSize++)
        {
            // Initializing max with minimum possible value
            int max=Integer.MIN_VALUE;
            
            // Traversing through all windows of current windowSize
            for(int i=0;i<=n-windowSize;i++)
            {
                // Checking for minimum element in current window
                int min=arr[i];
                for(int j=1;j<windowSize;j++)
                {
                    min=Math.min(min,arr[i+j]);
                }
                // Updating value of max if the minimum of this window is greater than max. 
                max=Math.max(max,min);
            }
            // Print max of min for all window of current windowSize
            System.out.print(max+" ");
        }
    }
	public static void main (String[] args) {
	    
		int arr[]={15, 20, 8, 30, 12, 20};
		
		printMaxofMinForEveryWindow(arr,arr.length);
	}
}




Python: 
INT_MIN = -9999999
def printMaxofMinForEveryWindow(arr, n):
     
    #Checking for windows of 
    #all valid sizes i.e. from 1 to n
    for windowSize in range(1, n + 1):
         
        # Initializing maxOfMin 
        # with very small value
        maxOfMin = INT_MIN;
 
        # Traversing through all 
        # windows of current windowSize
        for i in range(n - windowSize + 1):
             
            # Checking for minimum element in current window
            min = arr[i]
            for j in range(windowSize):
                if (arr[i + j] < min):
                    min = arr[i + j]
 
            # Updating value of max if minimum 
            # of this window is greater than max. 
            if (min > maxOfMin):
                maxOfMin = min
                 
        # Print max of min for all window of current windowSize
        print(maxOfMin, end = " ")
 
arr = [15, 20, 8, 30, 12, 20]
n = len(arr)
printMaxofMinForEveryWindow(arr, n)



Output: 
30 15 12 8 8 8


Complexity  Analysis:  
Time Complexity: O(n^3). 
Since 3 nested loops are used in the approach.
Extra Space: O(1) 
Because we are using variables only for storing starting and points of the windows.


Efficient Approach -
For each element, we can find what is the maximum size of the window in which that element is minimum that too in linear time complexity. We use this idea to build our algorithm.

Algorithm : 
Step 1 -
For each element find the index of the first smaller element on the right side (next smaller element) and on the left side (previous smaller element).
If there is no smaller element on the left side then the previous smaller is -1 and if there is no smaller on the right side then the next smaller is n. 
For input {15, 20, 8, 30, 12, 20}, array of indexes of previous smaller is {-1, 0, -1, 2, 2, 4}
For the input {15, 20, 8, 30, 12, 20}, array of indexes of next smaller is {2, 2, 6, 4, 6, 6}. 
To find these values it is described briefly here___________.

Step 2 -
As we have indexes of next smaller and previous smaller elements, by observation we can determine the length of the window in which a[i] is minimum is “right[i] - left[i] - 1”. 
Using the same we can find that length of windows for which a[i] is minimum can be represented by array {2, 1, 6, 1, 3, 1}. This means that the first element is minimum for a window size of 2, second for a window size of 1, and so on. 
Now we can create an auxiliary answer array of size n+1, ans[n+1] and fill the values according to values of left[] and right[]. 
    for (int i=0; i < n; i++)
    {
        // length of the interval
        int length = right[i] - left[i] - 1;

        // arr[i] is a possible answer for
        // this length length interval, so we store
        // max possible answer. 
        ans[length] = max(ans[length], arr[i]);
    }

Using this we get the ans[] array as {0, 30, 15, 12, 0, 0, 8}. Here ans[0] represents the maximum of minimum of the window of size 0 which is not required. 

Step 3: 
We can see that some elements in the answer array are 0 which means that they are yet to be filled. In the above case, we do not know the maximum of minimums of windows of length 4, and 5.  
We can make two observations here - 
a) Result for length i, i.e. ans[i] would always be greater or equal to result for length i+1, i.e., ans[i+1]. 
b) If ans[i] is not filled it means there is no single element which is minimum of the window of length i and therefore either the element of length ans[i+1], or ans[i+2], and so on is equal to ans[i] 
So we fill the remaining of the entries using the given for loop. 

    for (int i=n-1; i>=1; i--)
        ans[i] = max(ans[i+1], ans[i]);

Below is the implementation of the above algorithm. 

CPP: 
 #include <bits/stdc++.h>
using namespace std;

void printMaxofMinForEveryWindow(int arr[],int n){
    
    // Creating left and right array of size n,
    // And initializing with -1 and n respectively. 
    int left[n];
    int right[n];
    for(int i=0;i<n;i++)
    {
        left[i]=-1;
        right[i]=n;
    }
    // Fill left array using concept discuss here_______.
    stack<int> st;
    for(int i=0;i<n;i++)
    {
         while(!st.empty()&&arr[st.top()]>=arr[i])
         st.pop();
         if(st.empty()) left[i]=-1;
         else left[i]=st.top();
         
        st.push(i);
    }
    // Clear the array 
    while(!st.empty()) st.pop();
    
    // Fill right array using concept discuss here_______.
    for(int i=n-1;i>-1;i--)
    {
         while(!st.empty()&&arr[st.top()]>=arr[i])
         st.pop();
         if(st.empty()) right[i]=n;
         else right[i]=st.top();
         
        st.push(i);
    }
    
    //Creating answer array of size n+1 for all windows 
    //ranging from 0 to n-1 of which 0 size window is of no use
    // and initializing with 0.
    int ans[n+1];
    
    for(int i=0;i<=n;i++) 
    ans[i]=0;
    
    for(int i=0;i<n;i++)
    {
        // Finding length of window in which arr[i] is minimum possible element.
        int length=right[i]-left[i]-1;
        // arr[i] is a possible answer for this length 'len' interval, 
        //check if arr[i] is more than max for 'length'
        if(arr[i]>ans[length]) ans[length]=arr[i];
    }
    // Fill the values which are not 
    //filled yet by checking values of right side of array.
    for(int i=n-1;i>0;i--)
    {
        ans[i]=max(ans[i],ans[i+1]);
    }
    //Printing max of min for all window sizes from 1 to n.
    for(int i=1;i<=n;i++)
    cout<<ans[i]<<" ";
}
int main() {
	int arr[]={15, 20, 8, 30, 12, 20};
	int n = sizeof(arr)/sizeof(arr[0]);
	printMaxofMinForEveryWindow(arr,n);
	return 0;
}



JAVA: 

import java.util.*;
public class Main {
     public static int[] prevSmaller(int[] a) {
         
         // Finding index of first smaller element on left side, using Stacks. 
         // You can check it here ________.
        int n=a.length;
        int ans[]=new int[n];
        Stack<Integer> st=new Stack<>();
        for(int i=0;i<n;i++)
        {
             while(!st.isEmpty()&&a[st.peek()]>=a[i])
             st.pop();
             if(st.isEmpty()) ans[i]=-1;
             else ans[i]=st.peek();
             
            st.push(i);
        }
        return ans;
    }
     public static int[] nextSmaller(int[] a) {
         // Finding index of first smaller element on right side, using Stacks. 
         // You can check it here ________.
        int n=a.length;
        int ans[]=new int[n];
        Stack<Integer> st=new Stack<>();
        for(int i=n-1;i>-1;i--)
        {
             while(!st.isEmpty()&&a[st.peek()]>=a[i])
             st.pop();
             if(st.isEmpty()) ans[i]=n;
             else ans[i]=st.peek();
             
            st.push(i);
        }
        return ans;
    }
    public static void printMaxofMinForEveryWindow(int arr[],int n){
        
        //Finding index of first smaller element on left side.
        int left[]=prevSmaller(arr);
        //Finding index of first smaller element on left side.
        int right[]=nextSmaller(arr);
        
        //Creating answer array of size n+1 for all windows
        //ranging from 0 to n-1 of which 0 size window is of no use.
        int ans[]=new int[n+1];
        for(int i=0;i<n;i++)
        {
            // Finding length of window in which
            //arr[i] is minimum possible element.
            int length=right[i]-left[i]-1;
            // arr[i] is a possible answer for this length 'len' 
            //interval, check if arr[i] is more than max for 'length'
            if(arr[i]>ans[length]) ans[length]=arr[i];
        }
        
        // Fill the values which are not filled
        //yet by checking values of right side of array.
        for(int i=n-1;i>0;i--)
        {
            ans[i]=Math.max(ans[i],ans[i+1]);
        }
        //Printing max of min for all window sizes from 1 to n.
        for(int i=1;i<=n;i++)
        System.out.print(ans[i]+" ");
    }
	public static void main (String[] args) {
		int arr[]={15, 20, 8, 30, 12, 20};
		
		printMaxofMinForEveryWindow(arr,arr.length);
	}
}


Python: 
def printMaxofMinForEveryWindow(arr, n):
     
    s = [] # Used to find previous
           # and next smaller
 
    # Create left and right array 
    # to store previous and next smaller
    # element, initialize with -1 and n repectively. 
    left = [-1] * (n + 1)
    right = [n] * (n + 1)
 
    # Fill elements of left[] using logic discussed
    # here________
    for i in range(n):
        while (len(s) != 0 and
               arr[s[-1]] >= arr[i]):
            s.pop()
 
        if (len(s) != 0):
            left[i] = s[-1]
 
        s.append(i)
 
    # Clear the stack 
    # to use it again for 
    # filling right array.
    while (len(s) != 0):
        s.pop()
 
    # Fill elements of right[] using same logic
    for i in range(n - 1, -1, -1):
        while (len(s) != 0 and arr[s[-1]] >= arr[i]):
            s.pop()
 
        if(len(s) != 0):
            right[i] = s[-1]
 
        s.append(i)
 
    # Create and initialize answer array
    # of size n+1.
    ans = [0] * (n + 1)
    for i in range(n + 1):
        ans[i] = 0
 
    # Fill answer array by comparing minimums
    # of all. Lengths computed using left[]
    # and right[]
    for i in range(n):
         
        # Finding length of window in which 
        # arr[i] is minimum possible element.
        Length = right[i] - left[i] - 1
 
        # arr[i] is a possible answer for this
        # length 'len' interval,check if arr[i] 
        # is more than max for 'length'
        ans[Length] = max(ans[Length], arr[i])
 
    # Fill the values which are not 
    # filled yet by checking values
    # of right side of array.
    for i in range(n - 1, 0, -1):
        ans[i] = max(ans[i], ans[i + 1])
 
    # Print the result
    for i in range(1, n + 1):
        print(ans[i], end = " ")
 
# Driver Code
if __name__ == '__main__':
 
    arr = [15, 20, 8, 30, 12, 20]
    n = len(arr)
    printMaxofMinForEveryWindow(arr, n)

Output : 
30 15 12 8 8 8
Complexity Analysis:  

Time Complexity: O(n). 
Finding the Next smaller element and the previous smaller element takes linear time.

Extra Space: O(n). 
Using stack for calculating next and previous smaller and array to store the intermediate results.

Wrapping Up!

In this article, we have learned how to calculate the maximum of minimums for every possible window of valid size. We discussed two approaches in which we can solve this problem. This article also contains well-explained codes of both the approaches in the three most popular languages which are c++, Java and python along with their respective outputs attached to the article for a better understanding of a wide range of our readers. 

We sincerely hope that this article has walked you through some deep and important concepts of Arrays and Stack and you learned how we should approach such kinds of problems. 

Happy Learning!



# Second
Rearrange an array such that arr[i] = i 
We have been given an integer array of size n, where every element a[i] is such that, 0<=a[i]<=n-1. Some of the elements may be missing. If the element is missing then there will be -1 written at the place of that element. We have to rearrange the array in such a way that each element a[i]=i and if, i is missing then store -1 at that place. 

Example - 
Input - arr[] = {5, 4, -1, 3, 0, 2, -1}
Output - arr[] = {0, -1, 2, 3, 4, 5, -1} 

Simple Approach (Brute Force) - 
In this approach we will check for each i, 0<=i<=n-1 if it exists in the given array. If yes, then we will make a[i]=i. Now let’s look at the algorithm for better understanding. 
Algorithm: 
Run a for loop from 0 to n-1 to traverse for each number. 
Using a nested for loop, traverse through the array. 
If a number is found in an array then swap elements at a[i] position with a[j]. 
If a number is missing and -1 is written instead of it. Then it will be automatically replaced. 
At last, using a for loop, we iterate through the array and if for any element a[i]!=i then we will make a[i]=-1.

Below is the implementation of the above approach 

CPP: 
#include <bits/stdc++.h>
using namespace std;
void makeEveryElementEqualtoIndex(int a[],int n){
    // Traverse for each Number
    // from 0 to n-1.
    for(int i=0;i<n;i++)
    {
        // Now traverse the array.
        for(int j=0;j<n;j++)
        {
            // If i is present then 
            // Swap a[i] with a[j].
            if(a[j]==i)
            {
                int temp=a[j];
                a[j]=a[i];
                a[i]=temp;
                break;
            }
        }
    }
    // Traverse the array and check If
    // Any element != it's index
    // Then make a[i]=-1. 
    for(int i=0;i<n;i++)
    {
        if(a[i]!=i)
        {
            a[i]=-1;
        }
    }
}
int main() {
    
	int arr[]={5, 4, -1, 3, 0, 2, -1};
	
    int n=sizeof(arr)/sizeof(arr[0]);
    
    makeEveryElementEqualtoIndex(arr,n);
    
    for(int i=0;i<n;i++) 
    cout<<arr[i]<<" ";
    
    cout<<endl;
	return 0;
}

Output:
0 -1 2 3 4 5 -1 

JAVA:
import java.util.*;
public class Main {
    public static void makeEveryElementEqualtoIndex(int a[],int n){
        // Traverse for each Number
        // from 0 to n-1.
        for(int i=0;i<n;i++)
        {
            // Now traverse the array.
            for(int j=0;j<n;j++)
            {
                // If i is present then 
                // Swap a[i] with a[j].
                if(a[j]==i)
                {
                    int temp=a[j];
                    a[j]=a[i];
                    a[i]=temp;
                    break;
                }
            }
        }
        // Traverse the array and check If
        // Any element != it's index
        // Then make a[i]=-1. 
        for(int i=0;i<n;i++)
        {
            if(a[i]!=i)
            {
                a[i]=-1;
            }
        }
    }
	public static void main (String[] args) {
	    int arr[]={5, 4, -1, 3, 0, 2, -1};
	    int n=arr.length;
	    makeEveryElementEqualtoIndex(arr,n);
	    
	    for(int i=0;i<n;i++) 
	    System.out.print(arr[i]+" ");
	    
	    System.out.println();
	}
}
Output:
0 -1 2 3 4 5 -1 
Python:
def makeEveryElementEqualtoIndex(a, n):
     
    # Traverse for each Number
    # from 0 to n-1.
    for i in range(n):
        # Now traverse the array.
        for j in range(n):
 
            # If i is present then 
            # Swap a[i] with a[j].
            if (a[j] == i):
                a[j], a[i] = a[i], a[j]
 
    # Traverse the array and check If
    # Any element != it's index
    # Then make a[i]=-1. 
    for i in range(n):
         
        if (a[i] != i):
            a[i] = -1
 
    for i in range(n):
        print(a[i], end = " ")
    
    print()
 
# Driver Code
arr = [ 5, 4, -1, 3, 0, 2, -1 ]
n = len(arr)
 
makeEveryElementEqualtoIndex(arr, n);

Output:
0 -1 2 3 4 5 -1 
C#:
using System;

public class main{
    static void makeEveryElementEqualtoIndex(int[] a,int n){
        // Traverse for each Number
        // from 0 to n-1.
        for(int i=0;i<n;i++)
        {
            // Now traverse the array.
            for(int j=0;j<n;j++)
            {
                // If i is present then 
                // Swap a[i] with a[j].
                if(a[j]==i)
                {
                    int temp=a[j];
                    a[j]=a[i];
                    a[i]=temp;
                    break;
                }
            }
        }
        // Traverse the array and check If
        // Any element != it's index
        // Then make a[i]=-1. 
        for(int i=0;i<n;i++)
        {
            if(a[i]!=i)
            {
                a[i]=-1;
            }
        }
    }
	static public void Main (){
    	int[] arr={5, 4, -1, 3, 0, 2, -1};
	    int n=arr.Length;
	    makeEveryElementEqualtoIndex(arr,n);
	    
	    for(int i=0;i<n;i++) 
	    Console.Write(arr[i]+" ");
	    
	    Console.WriteLine();
	}
}
Output:
0 -1 2 3 4 5 -1 
Complexity Analysis:
Time Complexity - O(n^2), Since we have used two nested loops.
Space Complexity -  O(1)

Another Approach (Using Extra Space) -
In this approach, we will store elements in a frequency array or in the HashSet. Then, we will again traverse the array to check if the element i exist in the HashSet or not. 
Let’s have a look at the Algorithm to have a better understanding -
Algorithm: 
Traverse the array and store all elements in a HashSet. 
Again traverse from 0 to n-1 using the for loop, where n is the length of the array. And check if i exist in HashSet then make a[i]=i, else make a[i]=-1.

Below is the implementation of the above approach - 
CPP:
#include <bits/stdc++.h>
using namespace std;
void makeEveryElementEqualtoIndex(int a[],int n){
    unordered_set<int> hs;
    // Traverse the array and
    // Add elements to the set.
    for(int i=0;i<n;i++)
    hs.insert(a[i]);
    
   // Again traverse from i=0 -> i=n-1
    for(int i=0;i<n;i++)
    {
        // If Set contains i
        // Then make a[i]=i,
        // else a[i]=-1
        if(hs.find(i)==hs.end())
        a[i]=-1;
        else
        a[i]=i;
    }
}
int main() {
    
	int arr[]={5, 4, -1, 3, 0, 2, -1};
	
    int n=sizeof(arr)/sizeof(arr[0]);
    
    makeEveryElementEqualtoIndex(arr,n);
    
    for(int i=0;i<n;i++) 
    cout<<arr[i]<<" ";
    
    cout<<endl;
	return 0;
}

Output:
0 -1 2 3 4 5 -1 

JAVA: 
import java.util.*;
public class Main {
    public static void makeEveryElementEqualtoIndex(int a[],int n){
        HashSet<Integer> hs=new HashSet<>();
        
        // Traverse the array
        // and add each element
        // to the HashSet
        for(int i=0;i<n;i++)
        hs.add(a[i]);
        
        // Again traverse from i=0 -> i=n-1
        for(int i=0;i<n;i++)
        {
            // If HashSet contains i
            // Then make a[i]=i,
            // else a[i]=-1
            if(hs.contains(i))
            a[i]=i;
            else
            a[i]=-1;
        }
    }
	public static void main (String[] args) {
	    int arr[]={5, 4, -1, 3, 0, 2, -1};
	    int n=arr.length;
	    makeEveryElementEqualtoIndex(arr,n);
	    
	    for(int i=0;i<n;i++) 
	    System.out.print(arr[i]+" ");
	    
	    System.out.println();
	}
}
Output:
0 -1 2 3 4 5 -1 

Python:
def makeEveryElementEqualtoIndex(a, n):
     
    s = set()
     
    # Traverse the array and
    # Add elements to the set.
    for i in range(len(a)):
        s.add(a[i])
        
    # Again traverse from i=0 -> i=n-1
    for i in range(len(a)):
       
        # If Set contains i
        # Then make a[i]=i,
        # else a[i]=-1
        if i in s:
            a[i] = i
        else:
            a[i] = -1
    
    for i in range(n):
        print(a[i], end = " ")
    
    print()
 
# Driver Code
arr = [ 5, 4, -1, 3, 0, 2, -1 ]
n = len(arr)
 
makeEveryElementEqualtoIndex(arr, n);

Output:
0 -1 2 3 4 5 -1 
C#:
using System;
using System.Collections.Generic;
public class main{
    static void makeEveryElementEqualtoIndex(int[] a,int n){
       HashSet<int> hs=new HashSet<int>();
        
        // Traverse the array
        // and add each element
        // to the HashSet
        for(int i=0;i<n;i++)
        hs.Add(a[i]);
        
        // Again traverse from i=0 -> i=n-1
        for(int i=0;i<n;i++)
        {
            // If HashSet contains i
            // Then make a[i]=i,
            // else a[i]=-1
            if(hs.Contains(i))
            a[i]=i;
            else
            a[i]=-1;
        }
    }
	static public void Main (){
    	int[] arr={5, 4, -1, 3, 0, 2, -1};
	    int n=arr.Length;
	    makeEveryElementEqualtoIndex(arr,n);
	    
	    for(int i=0;i<n;i++) 
	    Console.Write(arr[i]+" ");
	    
	    Console.WriteLine();
	}
}
Output:
0 -1 2 3 4 5 -1
Complexity Analysis:
Time Complexity - O(n), We have traversed the array only two times.
Space Complexity -  O(n), Since we have used HashSet.


Optimized Approach: 
In this approach, we will try to solve the question in linear Time Complexity and in constant extra space. 

Algorithm: 
Traverse through the array. 
And if a[i] is not equal to -1 and it is not present at its correct position then we will swap it by its correct position i.e. a[a[i]].

Below is the implementation of the above approach - 

CPP:
#include <bits/stdc++.h>
using namespace std;
void makeEveryElementEqualtoIndex(int a[],int n){
 // Traverse the array 
    for (int i=0;i<n;)
    {
        // If a[i] is not equal 
        // to -1 and it is not at
        // its correct position.
        // Then swap with its correct
        // position
        if (a[i]!=-1&&a[i]!=i)
        {
            int temp=a[a[i]];
            a[a[i]]=a[i];
            a[i]=temp;
        }
        else
        {
            i++;
        }
    }
}
int main() {
    
	int arr[]={5, 4, -1, 3, 0, 2, -1};
	
    int n=sizeof(arr)/sizeof(arr[0]);
    
    makeEveryElementEqualtoIndex(arr,n);
    
    for(int i=0;i<n;i++) 
    cout<<arr[i]<<" ";
    
    cout<<endl;
	return 0;
}
Output:
0 -1 2 3 4 5 -1 

JAVA:
import java.util.*;
public class Main {
    public static void makeEveryElementEqualtoIndex(int a[],int n){
        
        // Traverse the array 
        for (int i=0;i<n;)
        {
            // If a[i] is not equal 
            // to -1 and it is not at
            // its correct position.
            // Then swap with its correct
            // position
            if (a[i]!=-1&&a[i]!=i)
            {
                int temp=a[a[i]];
                a[a[i]]=a[i];
                a[i]=temp;
            }
            else
            {
                i++;
            }
        }
    }
	public static void main (String[] args) {
	    int arr[]={5, 4, -1, 3, 0, 2, -1};
	    int n=arr.length;
	    makeEveryElementEqualtoIndex(arr,n);
	    
	    for(int i=0;i<n;i++) 
	    System.out.print(arr[i]+" ");
	    
	    System.out.println();
	}
}
Output:
0 -1 2 3 4 5 -1 

Python:
def makeEveryElementEqualtoIndex(a, n):
     
    i = 0
    # Traverse the array 
    while i < n:
        
        # If a[i] is not equal 
        # to -1 and it is not at
        # its correct position.
        # Then swap with its correct
        # position
        if (a[i] >= 0 and a[i] != i):
            (a[a[i]],a[i]) = (a[i],a[a[i]])
        else:
            i += 1
    
    for i in range(n):
        print(a[i], end = " ")
    
    print()
 
# Driver Code
arr = [ 5, 4, -1, 3, 0, 2, -1 ]
n = len(arr)
 
makeEveryElementEqualtoIndex(arr, n);
Output:
0 -1 2 3 4 5 -1 
C#:
using System;

public class main{
    static void makeEveryElementEqualtoIndex(int[] a,int n){
        // Traverse the array 
        for (int i=0;i<n;)
        {
            // If a[i] is not equal 
            // to -1 and it is not at
            // its correct position.
            // Then swap with its correct
            // position
            if (a[i]!=-1&&a[i]!=i)
            {
                int temp=a[a[i]];
                a[a[i]]=a[i];
                a[i]=temp;
            }
            else
            {
                i++;
            }
        }
    }
	static public void Main (){
    	int[] arr={5, 4, -1, 3, 0, 2, -1};
	    int n=arr.Length;
	    makeEveryElementEqualtoIndex(arr,n);
	    
	    for(int i=0;i<n;i++) 
	    Console.Write(arr[i]+" ");
	    
	    Console.WriteLine();
	}
}
Output:
0 -1 2 3 4 5 -1
Complexity Analysis:
Time Complexity - O(n), We only traversed the array once.
Space Complexity -  O(1), No extra space is used. 
Wrapping Up!

In this article, we have learned how to rearrange the array such that a[i]=i. We discussed three approaches in which we can solve this problem. This article also contains well-explained codes of both the approaches in the four most popular languages which are c++, Java, python and C# along with their respective outputs attached to the article for a better understanding of a wide range of our readers. 

We sincerely hope that this article has walked you through some deep and important concepts of Arrays and you have learned how we should approach such kinds of problems. We hope that you will now be able to solve this problem as well as its other variations seamlessly and efficiently.

Happy Coding!
