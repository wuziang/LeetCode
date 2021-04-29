# LeetCode

Accepted Submissions to Top Interview Questions

## 1. Two Sum
Related Topics: Array, Hash Table

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
       unordered_map<int, int> hash;
        
        for (int i=0;i<nums.size();i++){
            int j = nums[i];
            
            if (hash.find(j)!=hash.end()){
                return vector<int> ({hash[j], i});
            }
            else{
                hash[target-j]=i;
            }
        }
        
        return vector<int>();
    }
};
```

## 2. Add Two Numbers
Related Topics: Linked List, Math, Recursion

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *head = nullptr, *ptr = nullptr;
        
        int carry=0, sum=0;
        while (l1!=nullptr || l2!=nullptr || carry!=0){
            int temp = carry;
            
            if (l1!=nullptr){
                temp += l1->val;
                l1 = l1->next;
            }
            if (l2!=nullptr){
                temp += l2->val;
                l2 = l2->next;
            }

            carry = temp/10;
            sum = temp%10;
            
            if(head==nullptr){
                head = new ListNode(sum);
                ptr = head;
            }
            else{
                ptr->next = new ListNode(sum);
                ptr = ptr->next;
            }
        }
        
        return head;
    }
};
```

## 3. Longest Substring Without Repeating Characters
Related Topics: Hash Table, Two Pointers, String, Sliding Window

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int result = 0;
        
        int j=0;
        unordered_map<char, int> hash;
        for (int i=0;i<s.size();i++){
            char c=s[i];
            if (hash.find(c)!=hash.end()){
                j = max(hash[c]+1,j);
            }
            
            hash[c]=i;
            result=max(i-j+1, result);
        }
        
        return result;
    }
};
```

## 4. Median of Two Sorted Arrays
Related Topics: Array, Binary Search, Divide and Conquer

```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        if (nums1.size()>nums2.size()){
            swap(nums1, nums2);
        }
        
        int i=0, j=0, m=(int)nums1.size(), n=(int)nums2.size();
        
        int start=0, end=m;
        while(start<=end){
            i=(start+end)/2;
            j=(m+n)/2-i;
            
            if (i<m && j>0 && nums1[i]<nums2[j-1]){
                start=i+1;
            }
            else if (i>0 && j<n && nums1[i-1]>nums2[j]){
                end=i-1;
            }
            else
                break;
        }
        
        int right = (i==m)? nums2[j]: (j==n)? nums1[i]: min(nums1[i], nums2[j]);
        if((m+n)%2==1){
            return right;
        }
        else{
            int left = (i==0)? nums2[j-1]: (j==0)? nums1[i-1]: max(nums1[i-1], nums2[j-1]);
            return (left+right)/2.0;
        }
    }
};
```

## 5. Longest Palindromic Substring
Related Topics: String, Dynamic Programming

```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        int n = (int)s.size();
        vector<vector<bool>> dp(n, vector<bool>(n));
        
        string result = s.substr(0,1);
        for(int i=0;i<n;i++){
            for(int j=0;j<=i;j++){
                dp[i][j] = (s[i]==s[j]) && (i-j<=1 || dp[i-1][j+1]);
                
                if(dp[i][j] && (i-j+1)>result.size()){
                    result = s.substr(j, i-j+1);
                }
            }
        }
        
        return result;
    }
};
```

## 7. Reverse Integer
Related Topics: Math

```cpp
class Solution {
public:
    int reverse(int x) {
        int sign = x>0?1:-1;
        x=abs(x);
        
        int y=0;
        while(x>0){
            if(y>INT_MAX/10){
                return 0;
            }
            y=y*10+x%10;
            x/=10;
        }
        
        return sign*y;
    }
};
```

## 8. String to Integer (atoi)
Related Topics: Math, String

```cpp
class Solution {
public:
    int myAtoi(string s) {
        int i=0;
        
        while (i<s.size()){
            if(s[i]!=' ')
                break;
            else
                i++;
        }
        
        bool plus=true;
        if(s[i]=='+' || s[i]=='-'){
            plus=(s[i]=='+');
            i++;
        }
        
        int x=0;
        while(isdigit(s[i])){
            int j = s[i]-'0';
            
            if (plus){
                if(x<=INT_MAX/10 && INT_MAX-x*10>=j)
                    x=x*10+j;
                else
                    return INT_MAX;
                
            }
            else{
                if(x>=INT_MIN/10 && INT_MIN-x*10<=-j)
                    x=x*10-j;
                else
                    return INT_MIN;
            }
            i++;
        }
        
        return x;
    }
};
```

## 10. Regular Expression Matching
Related Topics: String, Dynamic Programming, Backtracking

```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        if (p.empty()){
            return s.empty();
        }
        
        int m=(int)s.size(), n=(int)p.size();
        
        string letters;
        vector<bool> stars;
        for (int i=0;i<n;i++){
            if(i+1<n && p[i+1]=='*'){
                letters.push_back(p[i]);
                stars.push_back(true);
                i++;
            }
            else{
                letters.push_back(p[i]);
                stars.push_back(false);
            }
        }
        
        int k=(int)letters.size();
        vector<vector<bool>> dp(k,vector<bool>(m+1,false));
        
        char c=letters[0];
        if(stars[0]){
            dp[0][0]=true;
            for (int j=1;j<=m;j++)
                dp[0][j] = dp[0][j-1] && (c==s[j-1] || c=='.');
        }
        else{
            dp[0][1] = (c==s[0] || c=='.');
        }
        
        for(int i=1;i<k;i++){
            c = letters[i];
            if(stars[i]){
                for(int j=0;j<=m;j++){
                    if(dp[i-1][j])
                        dp[i][j]=true;
                    else if (j>0 && (dp[i-1][j-1] || dp[i][j-1]))
                        dp[i][j] = (c==s[j-1] || c=='.');
                }
            }
            else{
                for(int j=1;j<=m;j++)
                    dp[i][j] = dp[i-1][j-1] && (c==s[j-1] || c=='.');
            }
        }
        
        return dp[k-1][m];
    }
};
```

## 11. Container With Most Water
Related Topics: Array, Two Pointers

```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int i=0, j=(int)height.size()-1;
        
        int left, right, area=0;
        
        while(i<j){
            left=height[i];
            right=height[j];
            
            area = max(area, min(left, right)*(j-i));
            
            if (left<right) {
                do{
                    i++;
                }while(i+1<=j && height[i]<=left);
            }
            else{
                do{
                    j--;
                }while(j-1>=i && height[j]<=right);
            }
        }
        
        return area;
    }
};
```

## 13. Roman to Integer
Related Topics: Math, String

```cpp
class Solution {
public:
    int romanToInt(string s) {
        unordered_map<char, int> hash({
            {'I', 1},
            {'V', 5},
            {'X', 10},
            {'L', 50},
            {'C', 100},
            {'D', 500},
            {'M', 1000},
        });
        
        int n=(int)s.size(), x=0;
        for (int i=0;i<n;i++){
            if(i+1<n && hash[s[i]]<hash[s[i+1]]){
                x-=hash[s[i]];
            }
            else{
                x+=hash[s[i]];
            }
        }
        
        return x;
    }
};
```

## 14. Longest Common Prefix
Related Topics: String

```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if(strs.empty()){
            return "";
        }
        
        int m=(int)strs.size(), n=(int)strs[0].size();
        for(int i=0;i<n;i++){
            char c=strs[0][i];
            
            for (int j=1;j<m;j++){
                if(i==strs[j].size() || c!=strs[j][i])
                    return strs[0].substr(0,i);
            }
        }
        
        return strs[0];
    }
};
```

## 15. 3Sum
Related Topics: Array, Two Pointers

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        
        sort(nums.begin(), nums.end());
        int n=(int)nums.size();
        
        for(int i=0;i<n;i++){
            int target = -nums[i], j=i+1, k=n-1;
            
            while(j<k){
                int sum = nums[j]+nums[k];
                
                if(sum<target){
                    j++;
                }
                else if(sum>target){
                    k--;
                }
                else{
                    result.push_back({nums[i], nums[j], nums[k]});
                    
                    int left = nums[j];
                    while(j<k && nums[j]==left)
                        j++;
                    
                    int right = nums[k];
                    while(k>j && nums[k]==right)
                        k--;
                }
            }
            
            while(i+1<n && nums[i]==nums[i+1]){
                i++;
            }
        }
        
        return result;
    }
};
```

## 17. Letter Combinations of a Phone Number
Related Topics: String, Backtracking, Depth-first Search, Recursion

```cpp
class Solution {
public:
    vector<string> letterCombinations(string digits) {
        vector<string> result;
        if(!digits.empty()){
            vector<string> mapping({"abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"});
            
            result.push_back("");
            for(char digit:digits){
                string letters = mapping[digit-'2'];
                
                vector<string> temp;
                for(char letter:letters){
                    for(string s:result){
                        s.push_back(letter);
                        temp.push_back(s);
                        s.pop_back();
                    }
                }
                result=temp;
            }
        }
        return result;
    }
};
```

## 19. Remove Nth Node From End of List
Related Topics: Linked List, Two Pointers

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode *first=head, *second=nullptr;
        
        while(first->next!=nullptr){
            first=first->next;
            
            if(n>0){
                if (n==1){
                    second=head;
                }
                n--;
            }
            else
                second=second->next;
        }
        
        if(second!=nullptr){
            second->next = second->next->next;
        }
        else{
            head = head->next;
        }
        
        return head;
    }
};
```

## 20. Valid Parentheses
Related Topics: String, Stack

```cpp
class Solution {
public:
    bool isValid(string s) {
        vector<char> temp;
        for (char i:s){
            if(i=='(' || i=='[' || i=='{'){
                temp.push_back(i);
            }
            else{
                if (temp.empty()) return false;
                
                char j=temp.back();
                temp.pop_back();
                
                switch (j) {
                    case '(':
                        if(i!=')') return false;
                        break;
                    case '[':
                        if(i!=']') return false;
                        break;
                    case '{':
                        if(i!='}') return false;
                        break;
                }
            }
        }
        
        return temp.empty();
    }
};
```

## 21. Merge Two Sorted Lists
Related Topics: Linked List, Recursion

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode *head=nullptr, *ptr=nullptr;
        
        while(l1!=nullptr || l2!=nullptr){
            if(l1==nullptr || (l2!=nullptr && l2->val<l1->val)){
                swap(l1, l2);
            }
            if (head==nullptr){
                head=l1;
                ptr=head;
            }
            else{
                ptr->next=l1;
                ptr=ptr->next;
            }
            l1=l1->next;
        }
        
        return head;
    }
};
```

## 22. Generate Parentheses
Related Topics: String, Backtracking

```cpp
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector<string> result;
        backtrack(result, "", 0, 0, n);
        
        return result;
    }
    void backtrack(vector<string>& result, string current, int open, int close, int n){
        if (current.size()==n*2){
            result.push_back(current);
            return;
        }
        
        if (open<n)
            backtrack(result, current+"(", open+1, close, n);
        if (close<open)
            backtrack(result, current+")", open, close+1, n);
    }
};
```

## 23. Merge k Sorted Lists
Related Topics: Linked List, Divide and Conquer, Heap

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        typedef priority_queue<ListNode*, vector<ListNode*>, comparator> priority_queue_ListNode;
        priority_queue_ListNode pq;
        
        for(ListNode* l:lists){
            if(l!=nullptr) pq.push(l);
        }
        
        ListNode *head=nullptr, *ptr=nullptr;
        while(!pq.empty()){
            ListNode* l=pq.top();
            pq.pop();
            
            if(head==nullptr){
                head=l;
                ptr=head;
            }
            else{
                ptr->next=l;
                ptr=ptr->next;
            }
            
            if(l->next!=nullptr)
                pq.push(l->next);
        }
        
        return head;
    }
private:
    struct comparator
    {
       bool operator()(const ListNode* a, const ListNode* b)
       {
           return a->val>b->val;
       }
    };
};
```

## 26. Remove Duplicates from Sorted Array
Related Topics: Array, Two Pointers

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int i=0, n=(int)nums.size();
        for (int j=0;j<n;j++){
            swap(nums[i], nums[j]);
            while (j+1<n && nums[j+1]==nums[i]){
                j++;
            }
            i++;
        }
        return i;
    }
};
```

## 28. Implement strStr()
Related Topics: Two Pointers, String

```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
        int m=(int)haystack.size(), n=(int)needle.size();
        for(int i=0;i+n<=m;i++){
            if(haystack.substr(i,n)==needle)
                return i;
        }
        return -1;
    }
};
```

## 29. Divide Two Integers
Related Topics: Math, Binary Search

```cpp
class Solution {
public:
    int divide(int dividend, int divisor) {
        if (dividend==INT_MIN && divisor==-1) return INT_MAX;
        
        long x=labs(dividend), y=labs(divisor), z=0;
        for (int i=31;i>=0;i--){
            if((x>>i)-y>=0){
                z += 1<<i;
                x -= y<<i;
            }
        }
        
        return (int)((dividend>0)==(divisor>0)? z: -z);
    }
};
```

## 33. Search in Rotated Sorted Array
Related Topics: Array, Binary Search

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int front=0, back=(int)nums.size()-1;
        while(front<=back){
            int mid=(front+back)/2;
            
            if(nums[mid]==target){
                return mid;
            }
            else if ((nums[front]<nums[mid] && (nums[front]<=target && target<=nums[mid]))){
                back=mid-1;
            }
            else if ((nums[front]>nums[mid] && (nums[front]<=target || target<=nums[mid]))){
                back=mid-1;
            }
            else{
                front=mid+1;
            }
        }
        return -1;
    }
};
```

## 34. Find First and Last Position of Element in Sorted Array
Related Topics: Array, Binary Search

```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int i=lower(nums, target), j=lower(nums, target+1)-1;
        
        if(i<nums.size() && nums[i]==target)
            return {i,j};
        else
            return {-1,-1};
    }
private:
    int lower(vector<int>& nums, int target){
        int front=0, back=(int)nums.size()-1;
        
        while(front<=back){
            int mid = (front+back)/2;
            
            if(nums[mid]<target)
                front=mid+1;
            else
                back=mid-1;
        }
        return front;
    }
};
```

## 36. Valid Sudoku
Related Topics: Hash Table

```cpp
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        for(int i=0;i<9;i++){
            bool row[9] = {false}, column[9] = {false}, box[9] = {false};
            
            for(int j=0;j<9;j++){
                int k;
                
                k = board[i][j]-'1';
                if (k>=0){
                    if(row[k]) return false;
                    row[k]=true;
                }
                
                k = board[j][i]-'1';
                if (k>=0){
                    if(column[k]) return false;
                    column[k]=true;
                }
                
                k = board[i/3*3+j/3][i%3*3+j%3]-'1';
                if (k>=0){
                    if(box[k]) return false;
                    box[k]=true;
                }
            }
        }
        return true;
    }
};
```

## 38. Count and Say
Related Topics: String

```cpp
class Solution {
public:
    string countAndSay(int n) {
        if (n==1){
            return "1";
        }
        else{
            string s=countAndSay(n-1);
            int m=(int)s.size();
            
            string result="";
            for(int i=0;i<m;i++){
                char c=s[i];
                int j=1;
                
                while(i+1<m && s[i+1]==c){
                    i++;
                    j++;
                }
                
                result.push_back(j+'0');
                result.push_back(c);
            }
            
            return result;
        }
    }
};
```

## 41. First Missing Positive
Related Topics: Array

```cpp
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int n=(int)nums.size();
        
        for(int i=0;i<n;i++){
            while(nums[i]>0 && nums[i]<=n && nums[i]!=nums[nums[i]-1])
                swap(nums[i], nums[nums[i]-1]);
        }
        
        for(int i=0;i<n;i++){
            if(nums[i]!=i+1)
                return i+1;
        }
        return n+1;
    }
};
```

## 42. Trapping Rain Water
Related Topics: Array, Two Pointers, Dynamic Programming, Stack

```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        int n=(int)height.size();
        
        vector<int> left(n), right(n);
        int maxLeft=0, maxRight=0;
        
        for(int i=0;i<n;i++){
            left[i]=maxLeft;
            right[n-1-i]=maxRight;
            
            maxLeft=max(maxLeft, height[i]);
            maxRight=max(maxRight, height[n-1-i]);
        }
        
        
        int result=0;
        for(int i=0;i<n;i++){
            result+=max(min(left[i], right[i])-height[i], 0);
        }
        
        return result;
    }
};
```

```cpp
cclass Solution {
public:
    int trap(vector<int>& height) {
        int result=0;
        
        int left=0, right=(int)height.size()-1;
        int maxLeft=0, maxRight=0;
        
        while(left<right){
            if(height[left]<height[right]){
                if(height[left]>maxLeft)
                    maxLeft=height[left];
                else
                    result+=(maxLeft-height[left]);
                left++;
            }
            else{
                if(height[right]>maxRight)
                    maxRight=height[right];
                else
                    result+=(maxRight-height[right]);
                right--;
            }
        }
        
        return result;
    }
};
```

## 44. Wildcard Matching
Related Topics: String, Dynamic Programming, Backtracking, Greedy

```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        if(p.empty())
            return s.empty();
        
        int m=(int)s.size(), n=(int)p.size();
        
        vector<vector<bool>> dp(n, vector<bool>(m+1, false));
        
        if(p[0]=='*')
            dp[0]=vector<bool>(m+1, true);
        else if (p[0]==s[0] || p[0]=='?')
            dp[0][1]=true;
        
        for(int i=1;i<n;i++){
            if(p[i]=='*'){
                dp[i][0]=dp[i-1][0];
                
                for(int j=1;j<m+1;j++){
                    dp[i][j]=(dp[i-1][j] || dp[i-1][j-1] || dp[i][j-1]);
                }
            }
            else{
                for(int j=1;j<m+1;j++){
                    dp[i][j]=(dp[i-1][j-1] && (p[i]==s[j-1] || p[i]=='?'));
                }
            }
        }
        
        return dp[n-1][m];
    }
};
```

## 46. Permutations
Related Topics: Backtracking

```cpp
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> result;
        
        permuteRecursive(result, nums, 0);
        return result;
    }
    
private:
    void permuteRecursive(vector<vector<int>>& result, vector<int>& nums, int i){
        int n=(int)nums.size();
        
        if(i>=n){
            result.push_back(nums);
            return;
        }
        
        for(int j=i;j<n;j++){
            swap(nums[i], nums[j]);
            permuteRecursive(result, nums, i+1);
            swap(nums[i], nums[j]);
        }
    }
};
```

## 48. Rotate Image
Related Topics: Array

```cpp
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = (int)matrix.size();
        
        for(int i=0;i<n/2;i++){
            int m=n-i*2;
            
            for(int j=0;j<m-1;j++){
                swap(matrix[i][i+j], matrix[i+j][n-1-i]);
                swap(matrix[i][i+j], matrix[n-1-i][n-1-(i+j)]);
                swap(matrix[i][i+j], matrix[n-1-(i+j)][i]);
            }
        }
    }
};
```

## 49. Group Anagrams
Related Topics: Hash Table, String

```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> hash;
        
        for (string str:strs){
            string key = str;
            
            sort(key.begin(), key.end());
            if(hash.find(key)!=hash.end())
                hash[key].push_back(str);
            else
                hash[key]={str};
        }
        
        vector<vector<string>> result;
        for (auto it=hash.begin();it!=hash.end();it++){
            result.push_back(it->second);
        }
        
        return result;
    }
};
```

## 50. Pow(x, n)
Related Topics: Math, Binary Search

```cpp
class Solution {
public:
    double myPow(double x, long n) {
        if(n==0)
            return 1;
        if(n<0){
            n = -n;
            x = 1/x;
        }
        
        double y=1;
        while(n>0){
            if(n&1) y*=x;
            x*=x;
            n>>=1;
        }
        
        return y;
    }
};
```

## 53. Maximum Subarray
Related Topics: Array, Divide and Conquer, Dynamic Programming

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int maxSum=nums[0], curSum=nums[0];
        
        for(int i=1;i<nums.size();i++){
            curSum = max(curSum+nums[i], nums[i]);
            maxSum = max(maxSum, curSum);
        }
        
        return maxSum;
    }
};
```

## 54. Spiral Matrix
Related Topics: Array

```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        int m=(int)matrix.size(), n=(int)matrix[0].size();
        
        vector<int> elements;
        
        for(int i=0;i<min((m+1)/2,(n+1)/2);i++){
            int x=m-i, y=n-i;
            
            for(int j=i;j<y;j++){
                elements.push_back(matrix[i][j]);
            }
            
            for(int j=i+1;j<x-1;j++){
                elements.push_back(matrix[j][y-1]);
            }

            if (i!=x-1){
                for(int j=y-1;j>=i;j--){
                    elements.push_back(matrix[x-1][j]);
                }
            }

            if (i!=y-1){
                for(int j=x-2;j>i;j--){
                    elements.push_back(matrix[j][i]);
                }
            }
        }
        
        return elements;
    }
};
```

## 55. Jump Game
Related Topics: Array, Greedy

```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int k=0;
        
        for(int i=0;i<nums.size();i++){
            if (k<i) return false;
            k = max(k, i+nums[i]);
        }
        
        return true;
    }
};
```

## 56. Merge Intervals
Related Topics: Array, Sort

```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<vector<int>> result;
        sort(intervals.begin(), intervals.end(), comparator);
        
        result.push_back(intervals[0]);
        for(int i=1;i<intervals.size();i++){
            vector<int> a=result.back(), b=intervals[i];
            
            if(a[1]<b[0]){
                result.push_back(b);
            }
            else{
                result.back()[0] = min(a[0], b[0]);
                result.back()[1] = max(a[1], b[1]);
            }
        }
        
        return result;
    }
private:
    static bool comparator(const vector<int>& a, const vector<int>& b){
        return a[0]<b[0];
    }
};
```

## 62. Unique Paths
Related Topics: Array, Dynamic Programming

```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m, vector<int>(n, 1));
        
        for (int i=1;i<m;i++){
            for(int j=1;j<n;j++){
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        
        return dp[m-1][n-1];
    }
};
```

## 66. Plus One
Related Topics: Array

```cpp
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        int n=(int)digits.size();
        
        for(int i=n-1;i>=0;i--){
            if(digits[i]==9){
                digits[i]=0;
            }
            else{
                digits[i]++;
                return digits;
            }
        }
        
        digits[0]=1;
        digits.push_back(0);
        
        return digits;
    }
};
```

## 69. Sqrt(x)
Related Topics: Math, Binary Search

```cpp
class Solution {
public:
    int mySqrt(int x) {
        if (x==0)
            return 0;
        
        int i=1, j=x/2;
        
        while (i<=j){
            int k=(i+j)/2;
            
            if(x/k<k){
                j=k-1;
            }
            else if(x/(k+1)>=(k+1)){
                i=k+1;
            }
            else
                return k;
        }
        
        return i;
    }
};
```

## 70. Climbing Stairs
Related Topics: Dynamic Programming

```cpp
class Solution {
public:
    int climbStairs(int n) {
        if(n==1)
            return 1;
        
        int a=1, b=2;
        
        for(int i=2;i<n;i++){
            int c = a+b;
            a = b;
            b = c;
        }
        
        return b;
    }
};
```

## 73. Set Matrix Zeroes
Related Topics: Array

```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        int m=(int)matrix.size(), n=(int)matrix[0].size();
        
        vector<bool> row(m, false), column(n, false);
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(matrix[i][j]==0){
                    row[i]=true;
                    column[j]=true;
                }
            }
        }
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(row[i] || column[j]){
                    matrix[i][j]=0;
                }
            }
        }
    }
};
```

## 75. Sort Colors
Related Topics: Array, Two Pointers, Sort

```cpp
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int n=(int)nums.size();
        
        int i=0, j=0, k=n-1;
        while(j<=k){
            if (nums[j]==2){
                swap(nums[k--], nums[j]);
            }
            else{
                if(nums[j]==0){
                    swap(nums[i++], nums[j]);
                }
                j++;
            }
        }
    }
};
```

## 76. Minimum Window Substring
Related Topics: Hash Table, Two Pointers, String, Sliding Window

```cpp
class Solution {
public:
    string minWindow(string s, string t) {
        string result="";
        
        int m=(int)t.size();
        unordered_map<char, int> hash;
        
        for(int i=0;i<m;i++){
            hash[t[i]]--;
        }
        
        int n=(int)s.size(), j=-1;
        for(int i=0;i<n;i++){
            if(hash.find(s[i])!=hash.end()){
                if(j==-1) j=i;
                
                if(hash[s[i]]++<0) m--;
                
                while(hash[s[j]]>0){
                    hash[s[j]]--;
                    do{
                        j++;
                    }while(hash.find(s[j])==hash.end());
                }
                
                if(m==0 && (result=="" || (i-j+1)<result.size())){
                    result=s.substr(j, i-j+1);
                }
            }
        }
        
        return result;
    }
};
```

## 78. Subsets
Related Topics: Array, Backtracking, Bit Manipulation

```cpp
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        int n=(int)nums.size();
        vector<vector<int>> result({{}});
        
        for(int i=0;i<n;i++){
            int m=(int)result.size();
            
            for(int j=0;j<m;j++){
                vector<int> subset=result[j];
                subset.push_back(nums[i]);
                result.push_back(subset);
            }
        }
        
        return result;
    }
};
```

## 79. Word Search
Related Topics: Array, Backtracking

```cpp
class Solution {
public:
    bool exist(vector<vector<char>>& board, string word) {
        m=(int)board.size();
        n=(int)board[0].size();
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(search(board, word, i, j, 0)) return true;
            }
        }
        
        return false;
    }
private:
    int m;
    int n;
    
    bool search(vector<vector<char>>& board, string& word, int x, int y, int i){
        if(x<0 || x>=m || y<0 || y>=n || board[x][y]!=word[i]) return false;
        
        if(i==word.size()-1) return true;
        
        char letter = board[x][y];
        board[x][y] = '*';
        
        bool found = search(board, word, x-1, y, i+1)
                      || search(board, word, x+1, y, i+1)
                      || search(board, word, x, y-1, i+1)
                      || search(board, word, x, y+1, i+1);
        
        board[x][y] = letter;
        return found;
    }
};
```

## 84. Largest Rectangle in Histogram
Related Topics: Array, Stack

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int result = 0;
        
        stack<int> s;
        heights.push_back(0);
        
        for(int i=0;i<heights.size();i++){
            while(!s.empty() && heights[s.top()]>=heights[i]){
                int j=s.top();
                s.pop();
                
                int k=(!s.empty())?s.top():-1;
                result=max(heights[j]*(i-k-1), result);
            }
            s.push(i);
        }
        
        return result;
    }
};
```

## 88. Merge Sorted Array
Related Topics: Array, Two Pointers

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i=m-1, j=n-1, k=m+n-1;
        
        while(i>=0 && j>=0){
            nums1[k--] = (nums1[i]>nums2[j])? nums1[i--]: nums2[j--];
        }
        
        while(j>=0){
            nums1[k--] = nums2[j--];
        }
    }
};
```

## 91. Decode Ways
Related Topics: String, Dynamic Programming

```cpp
class Solution {
public:
    int numDecodings(string s) {
        int n=(int)s.size();
        vector<int> dp(n, 0);
        
        dp[0]=(s[0]!='0');
        
        for(int i=1;i<n;i++){
            if(s[i]!='0'){
                dp[i]+= dp[i-1];
            }
            if(s[i-1]=='1' || (s[i-1]=='2' && s[i]<='6')){
                dp[i]+= (i>1)?dp[i-2]:1;
            }
        }
        
        return dp[n-1];
    }
};
```

## 94. Binary Tree Inorder Traversal
Related Topics: Hash Table, Stack, Tree

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root){
        vector<int> result;
        inorderRecursive(result, root);
        return result;
    }
private:
    void inorderRecursive(vector<int>& result, TreeNode* cur){
        if(cur==nullptr)
            return;
        
        inorderRecursive(result, cur->left);
        result.push_back(cur->val);
        inorderRecursive(result, cur->right);
    }
};
```

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        
        stack<TreeNode*> s;
        
        TreeNode* cur = root;
        while(cur!=nullptr || !s.empty()){
            while(cur!=nullptr){
                s.push(cur);
                cur=cur->left;
            }
            cur=s.top();
            s.pop();
            
            result.push_back(cur->val);
            cur=cur->right;
        }
        
        return result;
    }
};
```

## 98. Validate Binary Search Tree
Related Topics: Tree, Depth-first Search, Recursion

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        TreeNode* pre=nullptr;
        return validRecursive(root, pre);
    }
private:
    bool validRecursive(TreeNode* cur, TreeNode* & pre){
        if(cur==nullptr)
            return true;
        
        if(!validRecursive(cur->left, pre))
            return false;
        
        if(pre!=nullptr && pre->val >= cur->val)
            return false;
        
        pre=cur;
        return validRecursive(cur->right, pre);
    }
};
```

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        stack<TreeNode*> s;
        TreeNode *cur=root, *pre=nullptr;
        
        while(cur!=nullptr || !s.empty()){
            while(cur!=nullptr){
                s.push(cur);
                cur=cur->left;
            }
            cur=s.top();
            s.pop();
            
            if(pre!=nullptr && pre->val>=cur->val)
                return false;
            
            pre=cur;
            cur=cur->right;
        }
        
        return true;
    }
};
```

## 101. Symmetric Tree
Related Topics: Tree, Depth-first Search, Breadth-first Search

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        return symmetricRecursive(root, root);
    }
private:
    bool symmetricRecursive(TreeNode* t1, TreeNode* t2){
        if(t1==nullptr && t2==nullptr)
            return true;
        if(t1==nullptr || t2==nullptr)
            return false;
        return ((t1->val==t2->val)
                && symmetricRecursive(t1->left, t2->right)
                && symmetricRecursive(t1->right, t2->left));
    }
};
```

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        queue<TreeNode*> q;
        q.push(root);
        q.push(root);
        while(!q.empty()){
            TreeNode* t1=q.front();
            q.pop();
            
            TreeNode* t2=q.front();
            q.pop();
            
            if(t1==nullptr && t2==nullptr) continue;
            
            if(t1==nullptr || t2==nullptr) return false;
            
            if(t1->val!=t2->val) return false;
            
            q.push(t1->left);
            q.push(t2->right);
            
            q.push(t1->right);
            q.push(t2->left);
        }
        return true;
    }
};
```

## 102. Binary Tree Level Order Traversal
Related Topics: Tree, Breadth-first Search

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> result;
        
        queue<TreeNode*> level;
        if (root!=nullptr) level.push(root);
        
        while(!level.empty()){
            int n=(int)level.size();
            
            vector<int> values;
            for(int i=0;i<n;i++){
                TreeNode* node=level.front();
                level.pop();
                
                if(node!=nullptr){
                    values.push_back(node->val);
                    
                    if(node->left!=nullptr) level.push(node->left);
                    if(node->right!=nullptr) level.push(node->right);
                }
            }
            result.push_back(values);
        }
            
        return result;
    }
};
```

## 103. Binary Tree Zigzag Level Order Traversal
Related Topics: Stack, Tree, Breadth-first Search

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> result;
        
        queue<TreeNode*> level;
        if(root!=nullptr) level.push(root);
        
        while(!level.empty()){
            int n=(int)level.size();
            
            vector<int> values(n);
            for(int i=0;i<n;i++){
                TreeNode* node=level.front();
                level.pop();
                
                int j= result.size()%2==0? i: n-1-i;
                values[j]=node->val;
                
                if(node->left!=nullptr) level.push(node->left);
                if(node->right!=nullptr) level.push(node->right);
            }
            
            result.push_back(values);
        }
            
        return result;
    }
};
```

## 104. Maximum Depth of Binary Tree
Related Topics: Tree, Depth-first Search, Recursion

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int maxDepth(TreeNode* root){
        if(root==nullptr) return 0;
        return max(maxDepth(root->left), maxDepth(root->right))+1;
    }
};
```

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int maxDepth(TreeNode* root){
        int result=0;
        
        queue<TreeNode*> q;
        if(root!=nullptr) q.push(root);
        
        while(!q.empty()){
            int n=(int)q.size();
            for(int i=0;i<n;i++){
                TreeNode* node=q.front();
                q.pop();
                
                if(node->left!=nullptr) q.push(node->left);
                if(node->right!=nullptr) q.push(node->right);
            }
            result++;
        }
        
        return result;
    }
};
```

## 105. Construct Binary Tree from Preorder and Inorder Traversal
Related Topics: Array, Tree, Depth-first Search

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        return buildRecursive(preorder, inorder, 0, 0, (int)inorder.size()-1);
    }
private:
    TreeNode* buildRecursive(vector<int>& preorder, vector<int>& inorder, int i, int a, int b) {
        if(i>preorder.size()-1 || a>b) return nullptr;
        
        TreeNode* root = new TreeNode(preorder[i]);
        int j=a;
        
        while(j<b && inorder[j]!=root->val){
            j++;
        }
        
        root->left = buildRecursive(preorder, inorder, i+1, a, j-1);
        root->right = buildRecursive(preorder, inorder, i+(j-a)+1, j+1, b);
        
        return root;
    }
};
```

## 108. Convert Sorted Array to Binary Search Tree
Related Topics: Tree, Depth-first Search

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return sortedRecursive(nums, 0, (int)nums.size()-1);
    }
private:
    TreeNode* sortedRecursive(vector<int>& nums, int a, int b) {
        if(a>b) return nullptr;
        
        int i=(a+b)/2;
        TreeNode* root = new TreeNode(nums[i]);
        
        root->left = sortedRecursive(nums, a, i-1);
        root->right = sortedRecursive(nums, i+1, b);
        
        return root;
    }
```

## 116. Populating Next Right Pointers in Each Node
Related Topics: Tree, Depth-first Search, Breath-first Search

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;

    Node() : val(0), left(NULL), right(NULL), next(NULL) {}

    Node(int _val) : val(_val), left(NULL), right(NULL), next(NULL) {}

    Node(int _val, Node* _left, Node* _right, Node* _next)
        : val(_val), left(_left), right(_right), next(_next) {}
};
*/

class Solution {
public:
    Node* connect(Node* root) {
        if(root==nullptr) return root;
        
        Node *cur=root, *pre=nullptr;
        
        while(cur->left!=nullptr){
            pre=cur;
            
            while (cur!=nullptr){
                cur->left->next=cur->right;
                if(cur->next)
                    cur->right->next=cur->next->left;
                cur=cur->next;
            }
            
            cur=pre->left;
        }
        
        return root;
    }
};
```

## 118. Pascal's Triangle
Related Topics: Array

```cpp
class Solution {
public:
    vector<vector<int>> generate(int numRows) {
        if(numRows==0) return {};
        
        vector<vector<int>> result({{1}});
        
        for(int i=1;i<numRows;i++){
            vector<int> row({1});
            
            for(int j=0;j<i-1;j++){
                row.push_back(result[i-1][j]+result[i-1][j+1]);
            }
            row.push_back(1);
            result.push_back(row);
        }
        return result;
    }
};
```

## 121. Best Time to Buy and Sell Stock
Related Topics: Array, Dynamic Programming

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int minPrice=INT_MAX, result=0;
        for(int i=0;i<prices.size();i++){
            if (prices[i]<=minPrice)
                minPrice=prices[i];
            else
                result=max(prices[i]-minPrice, result);
        }
        return result;
    }
};
```

## 122. Best Time to Buy and Sell Stock II
Related Topics: Array, Greedy

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int result=0;
        for(int i=1;i<prices.size();i++){
            result+=max(prices[i]-prices[i-1],0);
        }
        return result;
    }
};
```

## 124. Binary Tree Maximum Path Sum
Related Topics: Tree, Depth-first Search, Recursion

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int maxPathSum(TreeNode* root) {
        int result=INT_MIN;
        pathRecursive(root, result);
        return result;
    }
private:
    int pathRecursive(TreeNode* node, int& result){
        int curMax=node->val, leftMax=0, rightMax=0;
        
        if(node->left!=nullptr)
            leftMax=max(pathRecursive(node->left, result),0);
        if(node->right!=nullptr)
            rightMax=max(pathRecursive(node->right, result),0);
        
        result=max(result, curMax+leftMax+rightMax);
        return curMax+max(leftMax, rightMax);
    }
};
```

## 125. Valid Palindrome
Related Topics: Two Pointers, String

```cpp
class Solution {
public:
    bool isPalindrome(string s) {
        int i=0, j=(int)s.size()-1;
        while(i<j){
            while(i<j && !isalnum(s[i]))
                i++;
            while(i<j && !isalnum(s[j]))
                j--;
            
            if(i<j && tolower(s[i++])!=tolower(s[j--]))
                return false;
        }
        return true;
    }
};
```

## 127. Word Ladder
Related Topics: Breadth-first Search

```cpp
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> hash(wordList.begin(), wordList.end());
        
        queue<string> q;
        q.push(beginWord);
        
        int result=1;
        while(!q.empty()){
            int n=(int)q.size();
            
            for(int i=0;i<n;i++){
                string word=q.front();
                q.pop();
                
                if(word==endWord) return result;
                
                for(int j=0;j<word.size();j++){
                    char c=word[j];
                    for(int k=0;k<26;k++){
                        word[j]='a'+k;
                        if(hash.find(word)!=hash.end()){
                            q.push(word);
                            hash.erase(word);
                        }
                    }
                    word[j] = c;
                }
            }
            result++;
        }
        
        return 0;
    }
};
```

## 128. Longest Consecutive Sequence
Related Topics: Array, Union Find

```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> hash(nums.begin(), nums.end());
        
        int result=0;
        for(auto it=hash.begin();it!=hash.end();it++){
            if(hash.find(*it-1)==hash.end()){
                int i=*it, n=1;
                
                while(hash.find(i+1)!=hash.end()){
                    i++;
                    n++;
                }
                
                result=max(n, result);
            }
        }
        
        return result;
    }
};
```

## 130. Surrounded Regions
Related Topics: Depth-first Search, Breath-first Search, Union Find

```cpp
class Solution {
public:
    void solve(vector<vector<char>>& board) {
        if(board.empty() || board[0].empty())
            return;
        if(board.size()<2 || board[0].size()<2)
            return;;
        
        m=(int)board.size();
        n=(int)board[0].size();
        
        for(int i=0;i<m;i++){
            if(board[i][0]=='O')
                boundaryRecursive(board, i, 0);
            if(board[i][n-1]=='O')
                boundaryRecursive(board, i, n-1);
        }
        
        for(int j=0;j<n;j++){
            if(board[0][j]=='O')
                boundaryRecursive(board, 0, j);
            if(board[m-1][j]=='O')
                boundaryRecursive(board, m-1, j);
        }
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(board[i][j]=='O')
                    board[i][j]='X';
                else if(board[i][j]=='*')
                    board[i][j]='O';
            }
        }
        
    }
private:
    int m;
    int n;
    void boundaryRecursive(vector<vector<char>>& board, int i, int j){
        if(i<0 || i>m-1 || j<0 || j>n-1)
            return;
        
        if(board[i][j]=='O')
            board[i][j]='*';
        
        if(i>1 && board[i-1][j]=='O')
            boundaryRecursive(board, i-1, j);
        
        if(i<m-2 && board[i+1][j]=='O')
            boundaryRecursive(board, i+1, j);
        
        if(j>1 && board[i][j-1]=='O')
            boundaryRecursive(board, i, j-1);
        
        if(j<n-2 && board[i][j+1]=='O')
            boundaryRecursive(board, i, j+1);
    }
};
```

## 131. Palindrome Partitioning
Related Topics: Dynamic Programming, Backtracking, Depth-first Search

```cpp
class Solution {
public:
    vector<vector<string>> partition(string s) {
        n=(int)s.size();
        vector<vector<string>> result;
        vector<vector<bool>> dp(n, vector<bool>(n, false));
        
        partitionRecursive(result, dp, s, {}, 0);
        return result;
    }
private:
    int n;
    void partitionRecursive(vector<vector<string>> &result, vector<vector<bool>> &dp, string &s, vector<string> a, int i) {
        if(i==n) result.push_back(a);
        
        for(int j=i;j<n;j++){
            if(s[i]==s[j] && (j-i<=2 || dp[i+1][j-1])){
                dp[i][j]=true;
                
                a.push_back(s.substr(i, j-i+1));
                partitionRecursive(result, dp, s, a, j+1);
                a.pop_back();
            }
        }
    }
};
```

## 134. Gas Station
Related Topics: Greedy

```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int n=(int)gas.size();
        
        int result=0, tank=0, total=0;
        for(int i=0;i<n;i++){
            tank+=gas[i]-cost[i];
            if(tank<0){
                tank=0;
                result=i+1;
            }
            
            total+=gas[i]-cost[i];
        }
        
        return (total>=0)?result:-1;
    }
};
```

## 136. Single Number
Related Topics: Hash Table, Bit Manipulation

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int result=0;
        for(int i=0;i<nums.size();i++) result ^=nums[i];
        return result;
    }
};
```

## 138. Copy List with Random Pointer
Related Topics: Hash Table, Linked List

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/

class Solution {
public:
    Node* copyRandomList(Node* head) {
        if(head==nullptr) return nullptr;
        
        Node* copyHead=new Node(head->val);
        
        unordered_map<Node*, Node*> hash;
        Node *cur=head, *copyCur=copyHead;
        hash[cur]=copyCur;
        
        while(cur!=nullptr){
            if(hash.find(cur->next)!=hash.end()){
                copyCur->next = hash[cur->next];
            }
            else if(cur->next!=nullptr){
                copyCur->next = new Node(cur->next->val);
                hash[cur->next] = copyCur->next;
            }
            
            if(hash.find(cur->random)!=hash.end()){
                copyCur->random = hash[cur->random];
            }
            else if(cur->random!=nullptr){
                copyCur->random = new Node(cur->random->val);
                hash[cur->random] = copyCur->random;
            }
            
            cur=cur->next;
            copyCur=copyCur->next;
        }
        
        return copyHead;
    }
};
```

## 139. Word Break
Related Topics: Dynamic Programming

```cpp
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> hash(wordDict.begin(), wordDict.end());
        
        int n=(int)s.size();
        vector<bool> dp(n+1, false);
        dp[0]=true;
        
        for(int i=1;i<=n;i++){
            for(int j=i-1;j>=0;j--){
                if(dp[j]){
                    string word = s.substr(j, i-j);
                    if(hash.find(word)!=hash.end()){
                        dp[i]=true;
                        break;
                    }
                }
            }
        }
        
        return dp[n];
    }
};
```

## 140. Word Break II
Related Topics: Dynamic Programming, Backtracking

```cpp
class Solution {
public:
    vector<string> wordBreak(string s, vector<string>& wordDict) {
        unordered_map<string, vector<string>> hash;
        return wordRecursive(s, wordDict, hash);
    }
private:
    vector<string> wordRecursive(string s, vector<string>& wordDict, unordered_map<string, vector<string>>& hash) {
        if(hash.find(s)!=hash.end()){
            return hash[s];
        }
        
        vector<string> result;
        
        if(s.length()==0){
            result.push_back("");
            return result;
        }
        
        for(string word:wordDict){
            if(s.substr(0, word.size())==word){
                vector<string> temp = wordRecursive(s.substr(word.size()), wordDict, hash);
                for(string t:temp){
                    result.push_back(word+(t.empty()?"":" ")+t);
                }
            }
        }
        
        hash[s]=result;
        return result;
    }
};
```

## 141. Linked List Cycle
Related Topics: Linked List, Two Pointers

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode *fast=head, *slow=head;
        
        while(fast!=nullptr && fast->next!=nullptr){
            fast=fast->next->next;
            slow=slow->next;
            
            if(fast==slow) return true;
        }
        
        return false;
    }
};
```

## 146. LRU Cache
Related Topics: Design

```cpp
class LRUCache {
public:
    LRUCache(int capacity) {
        n=capacity;
    }
    
    int get(int key) {
        LRUHash::iterator it = hash.find(key);
        
        if(it==hash.end()) return -1;
        
        update(it);
        return it->second.first;
    }
    
    void put(int key, int value) {
        LRUHash::iterator it = hash.find(key);
        
        if(it!=hash.end()){
            update(it);
        }
        else{
            if(hash.size()==n){
                hash.erase(least.back());
                least.pop_back();
            }
            
            least.push_front(key);
        }
        hash[key]={value, least.begin()};
    }
    
private:
    typedef list<int> LRUList;
    typedef unordered_map<int, pair<int, LRUList::iterator>> LRUHash;
    
    int n;
    LRUHash hash;
    LRUList least;
    
    void update(LRUHash::iterator it){
        int key = it->first;
        
        least.erase(it->second.second);
        
        least.push_front(key);
        it->second.second=least.begin();
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```

## 148. Sort List
Related Topics: Linked List, Sort

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        ListNode *ptr=head, *half=nullptr;
        
        while(ptr!=nullptr && ptr->next!=nullptr){
            ptr=ptr->next->next;
            
            if(half==nullptr){
                half=head;
            }
            else{
                half=half->next;
            }
        }
        
        if(half==nullptr) return head;
        
        ListNode *l1=head, *l2=half->next;
        
        half->next=nullptr;
        
        l1 = sortList(l1);
        l2 = sortList(l2);
        
        return mergeList(l1, l2);
    }
private:
    ListNode* mergeList(ListNode* l1, ListNode* l2){
        ListNode *head=nullptr, *cur=nullptr;
        
        while(l1!=nullptr || l2!=nullptr){
            if(l1==nullptr || (l2!=nullptr && l2->val<l1->val)) swap(l1, l2);
            
            if(head==nullptr){
                head=l1;
                cur=head;
            }
            else{
                cur->next=l1;
                cur=cur->next;
            }
            l1=l1->next;
        }
        
        return head;
    }
};
```

## 149. Max Points on a Line
Related Topics: Hash Table, Math

```cpp
class Solution {
public:
    int maxPoints(vector<vector<int>>& points) {
        int n=(int) points.size();
        if(n<=2) return n;
        
        int result=0;
        
        unordered_map<int, unordered_map<int, int>> hash;
        for(int i=0;i<n;i++){
            hash.clear();
            int count=0, overlap=1;
            
            for(int j=i+1;j<n;j++){
                int x=points[j][0]-points[i][0];
                int y=points[j][1]-points[i][1];
                
                if(x==0&&y==0){
                    overlap++;
                    continue;
                }
                
                int z=greatestCommonDivisor(x, y);
                if(z!=0){
                    x/=z;
                    y/=z;
                }
                
                if(hash.find(x)!=hash.end() && hash[x].find(y)!=hash[x].end()){
                    hash[x][y]++;
                }
                else{
                    hash[x][y]=1;
                }
                count=max(count, hash[x][y]);
            }
            
            result=max(result, count+overlap);
        }
        return result;
    }
private:
    int greatestCommonDivisor(int a, int b){
        if(b==0) return a;
        else return greatestCommonDivisor(b, a%b);
    }
};
```

## 150. Evaluate Reverse Polish Notation
Related Topics: Stack

```cpp
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<int> s;
        unordered_set<string> hash({"+","-", "*", "/"});
        for(string token:tokens){
            if(hash.find(token)!=hash.end()){
                int x=s.top();
                s.pop();
                int y=s.top();
                s.pop();
                
                switch (token[0]) {
                    case '+':
                        s.push(x+y);
                        break;
                    case '-':
                        s.push(y-x);
                        break;
                    case '*':
                        s.push(x*y);
                        break;
                    case '/':
                        s.push(y/x);
                        break;
                    default:
                        break;
                }
            }
            else{
                s.push(stoi(token));
            }
        }
        
        return s.top();
    }
};
```

## 152. Maximum Product Subarray
Related Topics: Array, Dynamic Programming

```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int result=nums[0];
        
        int n=(int)nums.size(), curMax=nums[0], curMin=nums[0];
        
        for(int i=1;i<n;i++){
            int p1=curMax*nums[i], p2=curMin*nums[i];
            
            curMax=max(nums[i], max(p1,p2));
            curMin=min(nums[i], min(p1,p2));
            
            result = max(result, curMax);
        }
        
        return result;
    }
};
```

## 155. Min Stack
Related Topics: Stack, Design

```cpp
class MinStack {
public:
    MinStack() {
        
    }
    
    void push(int x) {
        valueStack.push(x);
        if(minStack.empty() || x<=minStack.top()) minStack.push(x);
    }
    
    void pop() {
        int x=valueStack.top();
        valueStack.pop();
        if(x==minStack.top()) minStack.pop();
    }
    
    int top() {
        return valueStack.top();
    }
    
    int getMin() {
        return minStack.top();
    }
    
private:
    stack<int> valueStack;
    stack<int> minStack;
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(val);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->getMin();
 */
```

## 160. Intersection of Two Linked Lists
Related Topics: Linked List

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode *ptrA=headA, *ptrB=headB;
        while(ptrA!=ptrB){
            ptrA = ptrA?ptrA->next:headB;
            ptrB = ptrB?ptrB->next:headA;
        }
        return ptrA;
    }
};
```

## 162. Find Peak Element
Related Topics: Array, Binary Search

```cpp
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int a=0, b=(int)nums.size()-1;
        
        while(a<b){
            int i=(a+b)/2;
            if(nums[i]>nums[i+1])
                b=i;
            else
                a=i+1;
        }
        
        return a;
    }
};
```

## 166. Fraction to Recurring Decimal
Related Topics: Hash Table, Math

```cpp
class Solution {
public:
    string fractionToDecimal(long a, long b) {
        string result, fraction;
        
        if((a>0 && b<0)||(a<0 && b>0)){
            result.push_back('-');
        }
        
        a=labs(a);
        b=labs(b);
        result.append(to_string(a/b));
        
        a%=b;
        if(a!=0){
            result.push_back('.');
        }
        else{
            return result;
        }
        
        unordered_map<long, int> hash;
        while(a!=0 && hash.find(a)==hash.end()){
            hash[a]=(int)fraction.size();
            
            fraction.push_back((a*10)/b+'0');
            a=(a*10)%b;
        }
        
        if(a!=0){
            result.append(fraction.substr(0, hash[a])+"("+fraction.substr(hash[a])+")");
        }
        else
            result.append(fraction);
        
        return result;
    }
};
```

## 169. Majority Element
Related Topics: Array, Divide and Conquer, Bit Manipulation

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int result=nums[0], count=1;
        
        for(int i=1;i<nums.size();i++){
            if(result==nums[i])
                count++;
            else
                count--;
            
            if(count<0){
                result=nums[i];
                count=1;
            }
        }
        
        return result;
    }
};
```

## 171. Excel Sheet Column Number
Related Topics: Math

```cpp
class Solution {
public:
    int titleToNumber(string s) {
        int result=s[0]-'A'+1;
        for(int i=1;i<s.size();i++){
            result=(result*26)+(s[i]-'A'+1);
        }
        return result;
    }
};
```

## 172. Factorial Trailing Zeroes
Related Topics: Math

```cpp
class Solution {
public:
    int trailingZeroes(int n) {
        int result=0;
        
        while(n/5>0){
            result+=n/5;
            n/=5;
        }
        
        return result;
    }
};
```

## 179. Largest Number
Related Topics: Sort

```cpp
class Solution {
public:
    string largestNumber(vector<int>& nums) {
        vector<string> strs;
        for(int num:nums) strs.push_back(to_string(num));
        
        sort(strs.begin(), strs.end(), comparator);
        if(strs[0][0]=='0') return "0";
        
        string result;
        for(string str:strs) result+=str;
        
        return result;
    }
private:
    static bool comparator(const string a, const string b){
        return a+b>b+a;
    }
};
```

## 189. Rotate Array
Related Topics: Array

```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        k %= (int)nums.size();
        
        reverse(nums.begin(), nums.end());
        
        vector<int>::iterator it=nums.begin();
        advance(it, k);
        
        reverse(nums.begin(), it);
        reverse(it, nums.end());
    }
};
```

## 190. Reverse Bits
Related Topics: Bit Manipulation

```cpp
class Solution {
public:
    uint32_t reverseBits(uint32_t n) {
        uint32_t result = 0, k = 31;
        
        while(n!=0){
            result += (n&1) << k;
            n>>=1;
            k-=1;
        }
        
        return result;
    }
};
```

## 191. Number of 1 Bits
Related Topics: Bit Manipulation

```cpp
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int k=0;
        while(n>0){
            k+=(n&1);
            n>>=1;
        }
        return k;
    }
};
```

## 198. House Robber
Related Topics: Dynamic Programming

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int result=0;
        
        int n=(int)nums.size();
        vector<int> dp(n,0);
        
        for(int i=0;i<n;i++){
            if(i<2)
                dp[i]=nums[i];
            else if(i==2)
                dp[i]=nums[i]+dp[i-2];
            else
                dp[i]=nums[i]+max(dp[i-2], dp[i-3]);
            
            result=max(result,dp[i]);
        }
        
        return result;
    }
};
```

## 200. Number of Islands
Related Topics: Depth-first Search, Breath-first Search, Union Find

```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        m=(int)grid.size();
        n=(int)grid[0].size();
        
        int result=0;
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                if(grid[i][j]=='1'){
                    islandRecursive(grid, i, j);
                    result++;
                }
            }
        }
        
        return result;
    }
private:
    int m;
    int n;
    void islandRecursive(vector<vector<char>>& grid, int i, int j){
        if(i<0 || i>=m || j<0 || j>=n) return;
        
        grid[i][j]='2';
        
        if(i>0 && grid[i-1][j]=='1')
            islandRecursive(grid, i-1, j);
        
        if(i<m-1 && grid[i+1][j]=='1')
            islandRecursive(grid, i+1, j);
        
        if(j>0 && grid[i][j-1]=='1')
            islandRecursive(grid, i, j-1);
        
        if(j<n-1 && grid[i][j+1]=='1')
            islandRecursive(grid, i, j+1);
    }
};
```

## 202. Happy Number
Related Topics: Hash Table, Math

```cpp
class Solution {
public:
    bool isHappy(int n) {
        unordered_set<int> hash;
        
        while(n!=1){
            
            int k=0;
            while(n>0){
                k+=(n%10)*(n%10);
                n/=10;
            }
            
            if(hash.find(k)!=hash.end()){
                return false;
            }
            else{
                hash.insert(k);
                n=k;
            }
        }
        
        return true;
    }
};
```

## 204. Count Primes
Related Topics: Hash Table, Math

```cpp
class Solution {
public:
    int countPrimes(int n) {
        vector<bool> hash(n, false);
        
        int result=0;
        for(int i=2;i<n;i++){
            if(!hash[i]){
                result++;
                for(int j=2;i*j<n;j++){
                    hash[i*j]=true;
                }
            }
        }
        
        return result;
    }
};

```

## 206. Reverse Linked List
Related Topics: Linked List

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head!=nullptr && head->next!=nullptr){
            ListNode *pre=head, *cur=head->next;
            
            head=reverseList(cur);
            
            pre->next=nullptr;
            cur->next=pre;
        }
        
        return head; 
    }
};
```

## 207. Course Schedule
Related Topics: Depth-first Search, Breath-first Search, Graph, Topological Sort

```cpp
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> graph(numCourses);
        vector<int> degree(numCourses, 0);
        
        for(vector<int> pre:prerequisites){
            graph[pre[1]].push_back(pre[0]);
            degree[pre[0]]++;
        }
        
        vector<int> search;
        for(int i=0;i<numCourses;i++){
            if(degree[i]==0) search.push_back(i);
        }
        
        for(int i=0;i<search.size();i++){
            for(int j:graph[search[i]]){
                degree[j]--;
                if(degree[j]==0) search.push_back(j);
            }
        }
        
        return search.size()==numCourses;
    }
};
```

## 208. Implement Trie (Prefix Tree)
Related Topics: 

```cpp
class Trie {
public:
    Trie() {
        head=new Node();
    }
    
    void insert(string word) {
        Node* cur=head;
        for(int i=0;i<word.size();i++){
            int j=word[i]-'a';
            
            if(cur->next[j]==nullptr){
                cur->next[j]=new Node();
            }
            cur=cur->next[j];
        }
        cur->stop=true;
    }
    
    bool search(string word) {
        Node* tail=traverse(word);
        return (tail!=nullptr && tail->stop);
    }
    
    bool startsWith(string prefix) {
        return traverse(prefix)!=nullptr;
    }

private:
    struct Node{
        bool stop;
        vector<Node*> next;
        
        Node():stop(false), next(vector<Node*>(26,nullptr)){};
    };
    
    Node* head;
    
    Node* traverse(string s){
        Node* cur=head;
        for(int i=0;i<s.size();i++){
            int j=s[i]-'a';
            
            if(cur->next[j]==nullptr){
                return nullptr;
            }
            cur=cur->next[j];
        }
        return cur;
    }
};
/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */
```

## 210. Course Schedule II
Related Topics: Depth-first Search, Breath-first Search, Graph, Topological Sort

```cpp
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> graph(numCourses);
        vector<int> degree(numCourses, 0);
        
        for(vector<int> pre:prerequisites){
            graph[pre[1]].push_back(pre[0]);
            degree[pre[0]]++;
        }
        
        vector<int> order;
        for(int i=0;i<numCourses;i++){
            if(degree[i]==0) order.push_back(i);
        }
        
        for(int i=0;i<order.size();i++){
            for(int j:graph[order[i]]){
                degree[j]--;
                if(degree[j]==0) order.push_back(j);
            }
        }
        
        return order.size()==numCourses?order:vector<int>();
    }
};
```

## 212. Word Search II
Related Topics: Backtracking, Trie

```cpp
class Solution {
public:
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        vector<string> result;
        
        Node* head=new Node();
        for(string word:words){
            Node* cur=head;
            for(int i=0;i<word.size();i++){
                int j=word[i]-'a';
                
                if(cur->next[j]==nullptr){
                    cur->next[j]=new Node();
                }
                cur=cur->next[j];
            }
            
            cur->word=word;
        }
        
        m=(int)board.size();
        n=(int)board[0].size();
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                findRecursive(board, result, head, i, j);
            }
        }
        return result;
    }
    
private:
    int m;
    int n;
    
    struct Node{
        string word;
        vector<Node*> next;
        Node(): word(""), next(vector<Node*>(26,nullptr)){};
    };
    
    void findRecursive(vector<vector<char>>& board, vector<string>& result, Node* cur, int i, int j){
        if(i<0 || i>=m || j<0 || j>=n || board[i][j]=='*') return;
        
        int k=board[i][j]-'a';
        if(cur->next[k]==nullptr) return;
        
        cur=cur->next[k];
        
        if(cur->word!=""){
            result.push_back(cur->word);
            cur->word="";
        }
        
        board[i][j]='*';
        
        findRecursive(board, result, cur, i-1, j);
        findRecursive(board, result, cur, i+1, j);
        findRecursive(board, result, cur, i, j-1);
        findRecursive(board, result, cur, i, j+1);
        
        board[i][j]=k+'a';
    }
};
```

## 215. Kth Largest Element in an Array
Related Topics: Divide and Conquer, Heap

```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int, vector<int>, greater<int>> heap;
        
        for(int i=0;i<nums.size();i++){
            if(heap.size()<k || nums[i]>=heap.top()){
                if(heap.size()==k){
                    heap.pop();
                }
                heap.push(nums[i]);
            }
        }
        
        return heap.top();
    }
};
```

## 217. Contains Duplicate
Related Topics: Array, Hash Table

```cpp
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        unordered_set<int> hash;
        for(int num:nums){
            if(hash.find(num)!=hash.end()) return true;
            hash.insert(num);
        }
        return false;
    }
};
```

## 218. The Skyline Problem
Related Topics: Divide and Conquer, Heap, Binary Indexed Tree, Segment Tree, Line Sweep

```cpp
class Solution {
public:
    vector<vector<int>> getSkyline(vector<vector<int>>& buildings) {
        vector<vector<int>> result;
        priority_queue<vector<int>, vector<vector<int>>, comparator> heap;
        
        int i=0, x=0, y=-1, n=(int)buildings.size();
        
        while(i<n||!heap.empty()){
            x=heap.empty()?buildings[i][0]:heap.top()[1];
            
            if(i>=n || buildings[i][0]>x){
                while(!heap.empty()&&heap.top()[1]<=x){
                    heap.pop();
                }
            }
            else{
                x=buildings[i][0];
                while(i<n && buildings[i][0]==x){
                    heap.push({buildings[i][2], buildings[i][1]});
                    i++;
                }
            }
            
            y=heap.empty()?0:heap.top()[0];
            
            if(result.empty()||result.back()[1]!=y){
                result.push_back({x, y});
            }
        }
        return result;
    }
private:
    struct comparator
    {
        bool operator()(const vector<int>& a, const vector<int>& b)
        {
            return a[0]<b[0] || (a[0]==b[0]&&a[1]<b[1]);
        }
    };
};
```

## 227. Basic Calculator II
Related Topics: String, Stack

```cpp
class Solution {
public:
    int calculate(string s) {
        stack<int> nums;
        
        int num=0;
        char op='+';
        
        for(int i=0;i<s.size();i++){
            if(isdigit(s[i])) num=num*10+(s[i]-'0');
            
            if((!isdigit(s[i]) && !iswspace(s[i])) || i==s.size()-1){
                switch (op) {
                    case '+':
                        nums.push(num);
                        break;
                    case '-':
                        nums.push(-num);
                        break;
                    case '*':
                        nums.top()*=num;
                        break;
                    case '/':
                        nums.top()/=num;
                        break;
                    default:
                        break;
                }
                num=0;
                op=s[i];
            }
        }
        
        int result=0;
        while(!nums.empty()){
            result+=nums.top();
            nums.pop();
        }
        
        return result;
    }
};
```

## 230. Kth Smallest Element in a BST
Related Topics: 

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    int kthSmallest(TreeNode* root, int k) {
        stack<TreeNode*> s;
        while(root!=nullptr || !s.empty()){
            while(root!=nullptr){
                s.push(root);
                root=root->left;
            }
            root=s.top();
            s.pop();
            
            if(--k==0) return root->val;
            
            root=root->right;
        }
        return 0;
    }
};
```

## 234. Palindrome Linked List
Related Topics: Linked List, Two Pointers

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        ListNode *back=head, *half=head;
        
        stack<int> s;
        while(back!=nullptr && back->next!=nullptr){
            back=back->next->next;
            
            s.push(half->val);
            half=half->next;
        }
        
        if(back!=nullptr){
            half=half->next;
        }
        
        while(!s.empty()){
            if(half->val!=s.top()) return false;
            s.pop();
            
            half=half->next;
        }
        
        return true; 
    }
};
```

## 236. Lowest Common Ancestor of a Binary Tree
Related Topics: Tree

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        stack<TreeNode*> s1, s2;
        findAncestors(s1, root, p->val);
        findAncestors(s2, root, q->val);
        
        TreeNode* result=nullptr;
        while(!s1.empty() && !s2.empty()){
            if(s1.top()==s2.top()){
                result=s1.top();
                s1.pop();
                s2.pop();
            }
            else{
                break;
            }
        }
        return result;
    }
    
private:
    bool findAncestors(stack<TreeNode*>& s, TreeNode* p, int x) {
        if(p==nullptr) return false;
        
        if(p->val==x
           || findAncestors(s, p->left, x)
           || findAncestors(s, p->right, x)){
            s.push(p);
            return true;
        }
        
        return false;
    }
};
```

## 237. Delete Node in a Linked List
Related Topics: Linked List

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    void deleteNode(ListNode* node) {
        node->val = node->next->val;
        node->next = node->next->next;
    }
};
```

## 238. Product of Array Except Self
Related Topics: Array

```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n=(int)nums.size();
        vector<int> result(n,1);
        
        int prefix=1, suffix=1;
        for(int i=1;i<n;i++){
            prefix*=nums[i-1];
            suffix*=nums[n-i];
            
            result[i]*=prefix;
            result[n-i-1]*=suffix;
        }
        
        return result;
    }
};
```

## 239. Sliding Window Maximum
Related Topics: Heap, Sliding Window, Dequeue

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int n=(int)nums.size();
        priority_queue<pair<int, int>> heap;
        
        vector<int> result;
        for(int i=0;i<n;i++){
            heap.push({nums[i], i});
            
            if(i>=k-1){
                result.push_back(heap.top().first);
                
                int j=heap.top().second;
                if(i-j>=k-1){
                    while(!heap.empty() && heap.top().second<=j){
                        heap.pop();
                    }
                }
            }
        }
        
        return result;
    }
};
```

## 240. Search a 2D Matrix II
Related Topics: Binary Search, Divide and Conquer

```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m=(int)matrix.size(), n=(int)matrix[0].size();
        
        int i=0, j=n-1;
        while(i<m&&j>=0){
            if(matrix[i][j]<target)
                i++;
            else if(matrix[i][j]>target)
                j--;
            else
                return true;
                
        }
        return false;
    }
};
```

## 242. Valid Anagram
Related Topics: Hash Table, Sort

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        if(s.size()!=t.size()) return false;
        
        int hash[26] = {0};
        
        for(int i=0;i<s.size();i++){
            hash[s[i]-'a']++;
            hash[t[i]-'a']--;
        }
        for(int i=0;i<26;i++){
            if(hash[i]!=0) return false;
        }
        
        return true;
    }
};
```

## 268. Missing Number
Related Topics: Array, Math, Bit Manipulation

```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n=(int)nums.size();
        for(int i=0;i<n;i++){
            while(nums[i]<n && nums[i]!=i){
                swap(nums[i], nums[nums[i]]);
            }
        }
        
        for(int i=0;i<n;i++){
            if(nums[i]!=i) return i;
        }
        return n;
    }
};
```

## 279. Perfect Squares
Related Topics: Math, Dynamic Programming, Breadth-first Search

```cpp
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n);
        
        for(int i=0;i<n;i++){
            dp[i]=i+1;
        }
        
        for(int i=2;i*i<=n;i++){
            dp[i*i-1]=1;
            
            for(int j=i*i;j<n;j++){
                dp[j]=min(dp[j-i*i]+1, dp[j]);
            }
        }
        
        return dp[n-1];
    }
};
```

## 283. Move Zeroes
Related Topics: Array, Two Pointers

```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int i=0;
        
        for(int j=0;j<nums.size();j++){
            if(nums[j]!=0){
                swap(nums[i++], nums[j]);
            }
        }
    }
};
```

## 287. Find the Duplicate Number
Related Topics: Array, Two Pointers, Binary Search

```cpp
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int n=(int)nums.size();
        
        for(int i=0;i<n-1;i++){
            while(nums[i]-1!=i){
                if(nums[i]==nums[nums[i]-1]){
                    return nums[i];
                }
                else{
                    swap(nums[i], nums[nums[i]-1]);
                }
            }
        }
        
        return nums[n-1];
    }
};
```

## 289. Game of Life
Related Topics: Array

```cpp
class Solution {
public:
    void gameOfLife(vector<vector<int>>& board) {
        int m=(int)board.size(), n=(int)board[0].size();
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                
                int lives=-board[i][j];
                
                for(int x=i-1;x<=i+1;x++){
                    for(int y=j-1;y<=j+1;y++){
                        if(x>=0 && x<m && y>=0 && y<n)
                            lives+=(board[x][y]&1);
                    }
                }
                
                if(lives==3 || (lives==2 && board[i][j]==1)){
                    board[i][j]|=2;
                }
            }
        }
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                board[i][j]>>=1;
            }
        }
    }
};
```

## 295. Find Median from Data Stream
Related Topics: Heap, Design

```cpp
class MedianFinder {
public:
    MedianFinder() {
        
    }
    
    void addNum(int num) {
        low.push(num);
        
        high.push(low.top());
        low.pop();
        
        if(low.size()<high.size()){
            low.push(high.top());
            high.pop();
        }
    }
    
    double findMedian() {
        return low.size()==high.size()? (low.top()+high.top())/2.0: low.top();
    }
    
private:
    priority_queue<int> low;
    priority_queue<int, vector<int>, greater<int>> high;
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * MedianFinder* obj = new MedianFinder();
 * obj->addNum(num);
 * double param_2 = obj->findMedian();
 */
```

## 297. Serialize and Deserialize Binary Tree
Related Topics: Tree, Design

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Codec {
public:
    string serialize(TreeNode* root) {
        string result="";
        
        queue<TreeNode*> q;
        q.push(root);
        
        while(!q.empty()){
            int n=(int)q.size();
            
            bool empty=true;
            string encode="";
            
            for(int i=0;i<n;i++){
                TreeNode* ptr=q.front();
                q.pop();
                
                if(ptr==nullptr){
                    encode.append("null,");
                }
                else{
                    empty=false;
                    encode.append(to_string(ptr->val)+",");
                    
                    q.push(ptr->left);
                    q.push(ptr->right);
                }
            }
            
            if(!empty) result.append(encode);
        }
        
        if(!result.empty()) result.pop_back();
        return result;
    }
    
    TreeNode* deserialize(string data) {
        if(data.empty()) return nullptr;
        
        int i=0, n=(int)data.size();
        
        string node=read(data, i);
        TreeNode* root=new TreeNode(stoi(node));
        
        queue<TreeNode*> q;
        q.push(root);
        
        while(i<n){
            node=read(data, ++i);
            if(node!="null"){
                q.front()->left = new TreeNode(stoi(node));
                q.push(q.front()->left);
            }
            
            node=read(data, ++i);
            if(node!="null"){
                q.front()->right = new TreeNode(stoi(node));
                q.push(q.front()->right);
            }
            
            q.pop();
        }
        
        return root;
    }
    
private:
    string read(string& data, int& i){
        string node="";
        
        while(i<data.size() && data[i]!=','){
            node.push_back(data[i++]);
        }
        
        return node;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec ser, deser;
// TreeNode* ans = deser.deserialize(ser.serialize(root));
```

## 300. Longest Increasing Subsequence
Related Topics: Binary Search, Dynamic Programming

```cpp
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<int> result;

        for(int i=0;i<nums.size();i++){
            auto it=lower_bound(result.begin(), result.end(), nums[i]);
            if(it==result.end())
                result.push_back(nums[i]);
            else
                *it=nums[i];
        }

        return (int)result.size();
    }
};
```

## 315. Count of Smaller Numbers After Self
Related Topics: Binary Search, Divide and Conquer, Sort, Binary Indexed Tree, Segment Tree

```cpp
class Solution {
public:
    vector<int> countSmaller(vector<int>& nums) {
        vector<int> result;
        if(nums.empty()) return result;
        
        int n=(int)nums.size();
        multiset<>
        
        for(int i=n-2;i>=0;i--){
            
            result.push_back(insert(root, nums[i]));
        }
        
        reverse(result.begin(), result.end());
        
        return result;
    }
    
private:
    struct BSTNode{
        int value;
        int count=1;
        int smaller=0;
        
        BSTNode* left;
        BSTNode* right;
        BSTNode(int x) : value(x), left(nullptr), right(nullptr) {};
    };
    
    int insert(BSTNode* root, int x){
        int n=0;
        while(true){
            if(x<root->value){
                root->smaller++;
                
                if(root->left==nullptr){
                    root->left=new BSTNode(x);
                    break;
                }
                else{
                    root=root->left;
                }
            }
            else if(x>root->value){
                n+=root->smaller+root->count;
                
                if(root->right==nullptr){
                    root->right=new BSTNode(x);
                    break;
                }
                else{
                    root=root->right;
                }
            }
            else{
                n+=root->smaller;
                
                root->count++;
                break;
            }
        }
        
        return n;
    }
};
```

## 322. Coin Change
Related Topics: Dynamic Programming

```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int n) {
        if(n==0) return 0;
        
        vector<int> dp(n, INT_MAX);
        
        for(int i:coins){
            if(i>n) continue;
            
            dp[i-1]=1;
            for(int j=i;j<n;j++){
                if(dp[j-i]!=INT_MAX)
                    dp[j]=min(dp[j-i]+1, dp[j]);
            }
        }
        
        if(dp[n-1]!=INT_MAX)
            return dp[n-1];
        else
            return -1;
    }
};
```

## 324. Wiggle Sort II
Related Topics: Sort

```cpp
class Solution {
public:
    void wiggleSort(vector<int>& nums) {
        int n=(int)nums.size();
        
        vector<int> temp(nums.begin(), nums.end());
        sort(temp.begin(), temp.end());
        
        int i=(n-1)/2, j=n-1, k=0;
        while(k<n){
            nums[k++]=temp[i--];
            if(k<n){
                nums[k++]=temp[j--];
            }
        }
    }
};
```

## 326. Power of Three
Related Topics: Math

```cpp
class Solution {
public:
    bool isPowerOfThree(int n) {
        return n>0 && 1162261467%n==0;
    }
};
```

## 328. Odd Even Linked List
Related Topics: Linked List

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if(head==nullptr) return nullptr;
        
        ListNode *odd=head, *even=head->next;
        
        while(even!=nullptr && even->next!=nullptr){
            ListNode* cur=even->next;
            
            even->next=even->next->next;
            cur->next=odd->next;
            odd->next=cur;
            
            even=even->next;
            odd=odd->next;
        }
        
        return head;
    }
};
```

## 329. Longest Increasing Path in a Matrix
Related Topics: Depth-first Search, Topological Sort, Memoization

```cpp
class Solution {
public:
    int longestIncreasingPath(vector<vector<int>>& matrix) {
        m=(int)matrix.size();
        n=(int)matrix[0].size();
        
        int result=0;
        vector<vector<int>> memory(m,vector<int>(n, 0));
        
        for(int i=0;i<m;i++){
            for(int j=0;j<n;j++){
                result=max(result, pathRecursive(matrix, memory, i, j));
            }
        }
        
        return result;
    }
private:
    int m;
    int n;
    int pathRecursive(vector<vector<int>>& matrix, vector<vector<int>>& memory, int i, int j){
        if(memory[i][j]==0){
            int value=matrix[i][j], result=0;
            
            if(i>0 && matrix[i-1][j]>value)
                result=max(result, pathRecursive(matrix, memory, i-1, j));
            if(i<m-1 && matrix[i+1][j]>value)
                result=max(result, pathRecursive(matrix, memory, i+1, j));
            if(j>0 && matrix[i][j-1]>value)
                result=max(result, pathRecursive(matrix, memory, i, j-1));
            if(j<n-1 && matrix[i][j+1]>value)
                result=max(result, pathRecursive(matrix, memory, i, j+1));
            
            memory[i][j]=result+1;
        }
        
        return memory[i][j];
    }
};
```

## 334. Increasing Triplet Subsequence
Related Topics: Binary Search, Dynamic Programming

```cpp
class Solution {
public:
    bool increasingTriplet(vector<int>& nums) {
        vector<int> triplet;
        for(int num:nums){
            auto it=lower_bound(triplet.begin(), triplet.end(), num);
            
            if(it==triplet.end())
                triplet.push_back(num);
            else
                *it=num;
            
            if(triplet.size()==3) return true;
        }
        return false;
    }
};
```

## 341. Flatten Nested List Iterator
Related Topics: Stack, Design

```cpp
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * class NestedInteger {
 *   public:
 *     // Return true if this NestedInteger holds a single integer, rather than a nested list.
 *     bool isInteger() const;
 *
 *     // Return the single integer that this NestedInteger holds, if it holds a single integer
 *     // The result is undefined if this NestedInteger holds a nested list
 *     int getInteger() const;
 *
 *     // Return the nested list that this NestedInteger holds, if it holds a nested list
 *     // The result is undefined if this NestedInteger holds a single integer
 *     const vector<NestedInteger> &getList() const;
 * };
 */

class NestedIterator {
public:
    NestedIterator(vector<NestedInteger> &nestedList) {
        int n=(int)nestedList.size();
        
        for(int i=n-1;i>=0;i--){
            s.push(nestedList[i]);
        }
    }
    
    int next() {
        int x=s.top().getInteger();
        s.pop();
        
        return x;
    }
    
    bool hasNext() {
        while(!s.empty()){
            if(s.top().isInteger()) return true;
            
            NestedInteger cur=s.top();
            s.pop();
            
            vector<NestedInteger> nestedList=cur.getList();
            int n=(int)nestedList.size();
        
            for(int i=n-1;i>=0;i--){
                s.push(nestedList[i]);
            }
        }
        return false;
    }
    
private:
    stack<NestedInteger> s;
};

/**
 * Your NestedIterator object will be instantiated and called as such:
 * NestedIterator i(nestedList);
 * while (i.hasNext()) cout << i.next();
 */
```

## 344. Reverse String
Related Topics: Two Pointers, String

```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        int n=(int)s.size();
        for(int i=0;i<n/2;i++){
            swap(s[i], s[n-1-i]);
        }
    }
};
```

## 347. Top K Frequent Elements
Related Topics: Hash Table, Heap

```cpp
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> hash;
        for(int i:nums) hash[i]++;
        
        priority_queue<int, vector<int>, greater<>> heap;
        
        for(auto it=hash.begin();it!=hash.end();it++){
            if(heap.size()<k || heap.top()<it->second){
                heap.push(it->second);
            }
            
            if(heap.size()>k){
                heap.pop();
            }
        }
        
        vector<int> result;
        for(auto it=hash.begin();it!=hash.end();it++){
            if(it->second>=heap.top()) result.push_back(it->first);
        }
        
        return result;
    }
};
```

## 350. Intersection of Two Arrays II
Related Topics: Hash Table, Two Pointers, Binary Search, Sort

```cpp
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int, int> hash;
        for(int i:nums1) hash[i]++;
        
        vector<int> result;
        for(int i:nums2){
            if(hash.find(i)!=hash.end()){
                result.push_back(i);
                hash[i]--;
                
                if(hash[i]==0) hash.erase(i);
            }
        }
        
        return result;
    }   
};
```

## 371. Sum of Two Integers
Related Topics: Bit Manipulation

```cpp
class Solution {
public:
    int getSum(int a, int b) {
        unsigned int carry=(a&b);
        int sum=(a^b);
        
        if(carry==0) return sum;
        
        carry<<=1;
        return getSum(carry, sum);
    }
};
```

## 378. Kth Smallest Element in a Sorted Matrix
Related Topics: Binary Search, Heap

```cpp
class Solution {
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int n=(int)matrix.size();
        
        int low=matrix[0][0], high=matrix[n-1][n-1];
        
        while(low<high){
            int mid=(low+high)/2;
            
            int j=0;
            for(int i=0;i<n;i++){
                auto it=upper_bound(matrix[i].begin(), matrix[i].end(), mid);
                j+=(it-matrix[i].begin());
            }
            
            if(j<k){
                low=mid+1;
            }
            else{
                high=mid;
            }
        }
        
        return low;
    }
};
```

## 380. Insert Delete GetRandom O(1)
Related Topics: Array, Hash Table, Design

```cpp
class RandomizedSet {
public:
    RandomizedSet() {
        
    }
    
    bool insert(int val) {
        if(hash.find(val)!=hash.end()) return false;
        
        nums.push_back(val);
        hash[val]=(int)nums.size()-1;
        
        return true;
    }
    
    bool remove(int val) {
        if(hash.find(val)==hash.end()) return false;
        
        int i=hash[val];
        
        nums[i]=nums.back();
        hash[nums[i]]=i;
        
        nums.pop_back();
        hash.erase(val);
        
        return true;
    }
    
    int getRandom() {
        return nums[rand()%nums.size()];
    }
    
private:
    vector<int> nums;
    unordered_map<int, int> hash;
};

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet* obj = new RandomizedSet();
 * bool param_1 = obj->insert(val);
 * bool param_2 = obj->remove(val);
 * int param_3 = obj->getRandom();
 */
```

## 384. Shuffle an Array
Related Topics: Design

```cpp
class Solution {
public:
    Solution(vector<int>& nums) {
        n=(int)nums.size();
        orginal=nums;
    }

    vector<int> reset() {
        return orginal;
    }

    vector<int> shuffle() {
        vector<int> shuffled=orginal;
        
        for(int i=0;i<n;i++){
            swap(shuffled[i], shuffled[rand()%n]);
        }
        
        return shuffled;
    }
private:
    int n;
    vector<int> orginal;
};
```

## 387. First Unique Character in a String
Related Topics: Hash Table, String

```cpp
class Solution {
public:
    int firstUniqChar(string s) {
        int n=(int)s.size();
        vector<int> hash(26, n);
        
        for(int i=0;i<n;i++){
            int j=s[i]-'a';
            
            hash[j]=(hash[j]==n)?i:-1;
        }
        
        int result=n;
        for(int i=0;i<26;i++){
            if(hash[i]!=-1) result=min(result, hash[i]);
        }
        
        return result!=n?result:-1;
    }
};
```

## 395. Longest Substring with At Least K Repeating Characters
Related Topics: Divide and Conquer, Recursion, Sliding Window

```cpp
class Solution {
public:
    int longestSubstring(string s, int k) {
        int n=(int)s.size();
        return substringRecursive(s, 0, n, k);
    }
private:
    int substringRecursive(string& s, int a, int b, int k) {
        if((b-a)<k) return 0;
        
        int hash[26]={0};
        for(int i=a;i<b;i++) hash[s[i]-'a']++;
        
        for(int i=a;i<b;i++){
            if(hash[s[i]-'a']>=k) continue;
            
            int j=i+1;
            while(j<b && hash[s[j]-'a']<k) j++;
            
            return max(substringRecursive(s, a, i, k), substringRecursive(s, j, b, k));
        }
        
        return (b-a);
    }
};
```

## 412. Fizz Buzz
Related Topics: Math

```cpp
class Solution {
public:
    vector<string> fizzBuzz(int n) {
        vector<string> result;
        for(int i=1;i<=n;i++){
            if(i%3!=0 && i%5!=0){
                result.push_back(to_string(i));
            }
            else{
                string t="";
                if(i%3==0) t.append("Fizz");
                if(i%5==0) t.append("Buzz");
                result.push_back(t);
            }
        }
        return result;
    }
};
```

## 454. 4Sum II
Related Topics: Hash Table, Binary Search

```cpp
class Solution {
public:
    int fourSumCount(vector<int>& A, vector<int>& B, vector<int>& C, vector<int>& D) {
        unordered_map<int, int> hash;
        for(int i:A){
            for(int j:B){
                hash[-(i+j)]++;
            }
        }
        
        int result=0;
        for(int i:C){
            for(int j:D){
                if(hash.find(i+j)!=hash.end())
                    result+=hash[i+j];
            }
        }
        
        return result;
    }
};
```
