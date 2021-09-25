# 排序算法
public class MySort {

    //BubbleSort

    public static void BubbleSort(int[] arr) {
        // boolean flag = true 到底放哪？
        //如果放在外面，即使中间有序了，也return不了
        for (int j = 0; j < arr.length - 1; j++) {
            boolean flag = true;//每次遍历前都认为它是有序的
            for (int i = 1; i < arr.length - j; i++) {
                if (arr[i] < arr[i - 1]) {
                    swap(arr, i, i - 1);
                    //flag = false 到底放哪？
                    // 如果放在else中，有一次比较成功就会反回
                    flag = false;
                }
            }
            if (flag) return;
        }
    }

    //MergeSort

    public static void MergeSort(int[] arr, int l, int r) {
        //递归终止条件 小细节1
        // 若l==r =》mid=l=r =》MergeSort(arr,mid+1,r)不成立
        if (l >= r) return;
        int mid = l + (r - l) / 2;//防止数据溢出 小细节2
        MergeSort(arr, l, mid);
        MergeSort(arr, mid + 1, r);
        //判断局部大小 优化
        //merge应传入mid 不断递归是mid是不同的 如果只用一个mid会出错 小细节3
        if (arr[mid] > arr[mid + 1]) merge(arr, l, mid, r);
    }

    private static void merge(int[] arr, int l, int mid, int r) {
        int[] temp = new int[arr.length];
        //开一个临时数组 用来运算
        for (int i = l; i <= r; i++) {
            temp[i] = arr[i];
        }
        int i = l, j = mid + 1;
        for (int k = l; k <= r; k++) {
            //经典操作是四步判断 可以合成为三步（如下） 但不能合成两步 小细节4
            if (i == mid + 1) arr[k] = temp[j++];
            else if (j == r + 1) arr[k] = temp[i++];
            //要使用temp进行比较 arr是合成之后的数组 不应拿来比较
            else if (temp[i] <= temp[j]) arr[k] = temp[i++];//使用 <= 来保证排序的稳定性
            else arr[k] = temp[j++];
        }
    }

//        public static void merge(int[] arr,int l,int mid,int r){
//        int[] temp = new int[arr.length];
//        for (int i = l; i <= r; i++) {
//            temp[i] = arr[i];
//        }
//        int i = l;
//        int j = mid + 1;
//        for (int k = l; k <= r; k++) {
//
//            if (i == mid + 1 ) {
//                arr[k] = temp[j];
//                j++;
//            } else if (j == r + 1 || temp[i] <= temp[j]) {
//                arr[k] = temp[i];
//                i++;
//            } else {
//                arr[k] = temp[j];
//                j++;
//            }
//        }
//    }

    //QuickSort

    public static void QuickSort(int[] arr, int l, int r) {
        //递归必须要有终止条件
        if(l>=r) return;
        //注意partition参数不应确定 会随着递归不断更新 小细节1
        int par = partition(arr, l, r);
        //[l,p-1] [p+1,r]继续递归 小细节2
        QuickSort(arr,l,par-1);
        QuickSort(arr,par+1,r);
    }

    private static int partition(int[] arr, int l, int r) {
        //arr[l+1..j]<=v   arr[j+1..]>=v
        int i=l+1,j=r;
        while(true){
            //假设最左边元素为标点元素
            while(i<=j && arr[i]<arr[l]) i++;
            while(i<=j && arr[j]>arr[l]) j--;
            //注意循环须有终止条件
            //一定要先判断是否越界 如果越界又交换 数据错误 小细节3
            if(i>=j) break;
            swap(arr,i,j);
            //注意如果来到等于区域 必须i++ j-- 否则会陷入死循环 小细节4
            i++;
            j--;
        }
        //j是小于部分的最后一个元素 所以标点元素与之交换 左边依然满足小于区域
        //如果交换大于部分的第一个元素 左边不满足小于区域 小细节5
        swap(arr,l,j);
        return j;
    }

//    private int partition(int[] arr, int l, int r) {
//        //优化
//        int p = l + rd.nextInt(r - l + 1);
//        swap(arr, l, p);
//        int  i = l + 1,j = r;
//        while (true) {
//            while (i <= j && arr[i] < arr[l]) {
//                i++;
//            }
//            while (j >= i && arr[j] > arr[l]) {
//                j--;
//            }
//            if (i >= j)
//                break;
//            swap(arr, i, j);
//            i++;
//            j--;
//        }
//        swap(arr, l, j);
//
//        return j;
//    }

    //BinarySearch

    public static int BinarySearch(int[] arr,int n) {
        // 注意边界控制

        int l=0,r=arr.length - 1;//r=arr.length? [0,arr.length-1]是闭区间

        // 如何判断l<=r 还是l<r?
        // 由于r=arr.length-1 如果只有一个数 l==r 此时while(l<r)不成立 若要取这个数则会return -1
        while(l<=r) {
            int mid = l + (r - l) / 2;
            if(arr[mid]==n){
                return mid;
            }else if(arr[mid]> n){
                r=mid-1;//r = mid ? 因为是闭区间 所以能取到最右边 不能包含mid
            }else{
                l=mid+1;//l = mid ? 因为是闭区间 所以能取到最左边 不能包含mid
            }
        }
        return -1;

    }
    private static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
