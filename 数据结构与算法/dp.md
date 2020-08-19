# dp

1. #### [ 打家劫舍](https://leetcode-cn.com/problems/house-robber/)

   ```java
   //递推方程：f(i) = Math.max(f(i-2)+f(i),f(i-1));
   class Solution {
       public int rob(int[] nums) {
           if(nums.length == 0){
               return 0;
           }
           if(nums.length < 2){
               return nums[0];
           }
   
           nums[1] = Math.max(nums[0],nums[1]);
   
           for(int i = 2;i < nums.length;i++){
               nums[i] = Math.max(nums[i-2]+nums[i],nums[i-1]);
           }
   
           return nums[nums.length-1];
       }
   }
   ```

   

2. #### [ 打家劫舍 II](https://leetcode-cn.com/problems/house-robber-ii/)

   ```java
   class Solution {
       //两次dp
       public int rob(int[] nums) {
           if(nums.length == 0){
               return 0;
           }
   
           if(nums.length == 1){
               return nums[0];
           }
           if(nums.length == 2){
               return Math.max(nums[0],nums[1]);
           }
   
           int[] nums1 = new int[nums.length-1];
           int[] nums2 = new int[nums.length];
   
           nums1[0] = nums[0];
           nums1[1] = Math.max(nums[0],nums[1]);
           for(int i = 2;i < nums1.length;i++){
               nums1[i] = Math.max(nums1[i-2]+nums[i],nums1[i-1]);
           }
   
           nums2[1] = nums[1];
           nums2[2] = Math.max(nums[1],nums[2]);
           for(int i = 3;i < nums2.length;i++){
               nums2[i] = Math.max(nums2[i-2]+nums[i],nums2[i-1]);
           }
   
           return Math.max(nums1[nums1.length-1],nums2[nums2.length-1]);
       }
   }
   ```

   

3. 

