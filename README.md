# Unbit-assignment
Unbit assignment
1 . Given an array of integers and a target value, you must determine which two integers' sum
equals the target and return a 2D array. Then merge the array into a single array with sorting (
ascending ) order, in the next step double the target value and find again the combination of
digits (can be multiple digits ) that are equal to the double targeted value and returned into a 2D
array.
Sample Input : [1, 3, 2, 2, -4, -6, -2, 8];
Target Value = 4,
Output: First Combination For “4” : [ [1,3],[2,2],[-4,8],[-6,2] ];
Merge Into a single Array : [-6,-4,1,2,2,2,3,8];
Second Combination For “8” : [ [ 1,3,2,2], [8,-4,2,2],....,[n,n,n,n] ]

Solution:
let array=[1, 3, 2, 2, -4, -6, -2, 8];
let target=4;

//first combination for 4 using brute force method
//its time complexity is O(n^2) because of nested loop
//its space complexity is O(n) because of result array

function findSumBruteForce(arr,target){
    let result = [];
    for(let i=0;i<arr.length;i++){
        for(let j=i+1;j<arr.length;j++){
            if(arr[i]+arr[j]===target){
                result.push([arr[i],arr[j]]);
            }
        }
    }
    return result;
}

// console.log(findSumBruteForce([1, 3, 2, 2, -4, -6, -2, 8],4));


//find combination for 4 using set method
//its time complexity is O(n) because of set
//its space complexity is O(n) because of set


function findSumSet(arr,target){
    let result = [];
    let set = new Set();
    for(let i=0;i<arr.length;i++){
        let first = arr[i];
        let second = target-first;
        if(set.has(second)){
            result.push([first,second]);
        }
        set.add(first);
    }
    console.log(`First Combination For "${target}" : `,result);
    merge(result,target);
    //return result;
}

// console.log(findSumSet([1, 3, 2, 2, -4, -6, -2, 8],4));


function findSumTwoPointer(arr,target){
    let result = [];
    let left = 0;
    let right = arr.length-1;
    arr.sort((a,b)=>a-b);
    while(left<right){
        let sum = arr[left]+arr[right];
        if(sum===target){
            result.push([arr[left],arr[right]]);
            left++;
            right--;
        }else if(sum<target){
            left++;
        }else{
            right--;
        }
    }
    return result;
}

// console.log(findSumTwoPointer([1, 3, 2, 2, -4, -6, -2, 8],4));

//merge into a single array


function merge(arr,target){
    let result = [];
    for(let i=0;i<arr.length;i++){
        result.push(...arr[i]);
    }
    console.log(`Merge Into a single Array : `,result);
    result.sort((a,b)=>a-b);
    console.log(`Second Combination For "${target*2}" : `,secondCombination(0,result,target*2,0));
    //return result;
}

// console.log(merge([[1,3],[2,2],[-4,8],[-6,2]]));

//second combination for 8 using recursion

let results = [];
let currcomb=[];
function secondCombination(index,arr,target,sum){
    if(sum===target){
        results.push([...currcomb]);
        return;
    }
    if(sum>target || index>=arr.length){
        return;
    }

    for(let i=index;i<arr.length;i++){
        sum+=arr[i];
        currcomb.push(arr[i]);
        secondCombination(i+1,arr,target,sum);
        currcomb.pop();
        sum-=arr[i];
    }
    return results;
}

// console.log(secondCombination(0,[-6,-4,1,2,2,2,3,8],8,0));

findSumSet(array,target);
