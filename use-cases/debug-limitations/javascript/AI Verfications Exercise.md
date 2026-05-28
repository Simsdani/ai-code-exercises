AI Verification Exercise — Sorting Function Bug
Selected Scenario
A sorting function with a subtle bug.
The chosen example was a Merge Sort implementation with an error inside the merge() function.
Step 1 — Asking AI to Solve the Problem
Original Buggy Code
while (i < left.length) {
    result.push(left[i]);
    j++; // incorrect variable increment
}
The AI identified that:
the wrong variable was being incremented
i should increase instead of j
Suggested fix:
while (i < left.length) {
    result.push(left[i]);
    i++;
}
Step 2 — Verification Strategies
1. Collaborative Solution Verification
I verified the AI solution by manually reviewing:
how merge sort works
the purpose of variables i and j
how array traversal should happen
I compared the corrected code with standard merge sort implementations online and confirmed that:
i tracks the left array
j tracks the right array
incrementing i was the correct fix
What I Learned
AI-generated solutions should still be reviewed carefully because:
small mistakes can completely break algorithms
understanding the logic is more important than copying fixes
2. Learning Through Alternative Approaches
I explored another way to solve the same problem by rewriting the merge logic with more descriptive variable names.
Alternative version:
let leftIndex = 0;
let rightIndex = 0;
This made the code easier to understand and reduced confusion between variables.
I also considered using JavaScript’s built-in sorting:
array.sort((a, b) => a - b);
but merge sort was better for learning algorithm concepts.
What I Learned
Exploring alternative implementations helped me:
better understand merge sort
improve readability
see how naming conventions reduce bugs
3. Developing a Critical Eye
Instead of assuming the AI was correct,
I checked:
loop conditions
variable updates
edge cases
possible infinite loops
I tested the code mentally using:
[5, 2, 1]
and followed the movement of i and j.
This helped confirm that:
i never changed in the buggy version
the loop condition would remain true forever
What I Learned
AI responses can sound correct even when subtle issues still exist.
It is important to:
trace the logic step-by-step
test edge cases
understand why the fix works
Step 3 — Final Verified Solution
Corrected Merge Sort Implementation
function mergeSort(arr) {
    if (arr.length <= 1) return arr;
    const mid = Math.floor(arr.length / 2);
    const left = mergeSort(arr.slice(0, mid));
    const right = mergeSort(arr.slice(mid));
    return merge(left, right);
}
function merge(left, right) {
    let result = [];
    let i = 0;
    let j = 0;
    while (i < left.length && j < right.length) {
        if (left[i] < right[j]) {
            result.push(left[i]);
            i++;
        } else {
            result.push(right[j]);
            j++;
        }
    }
    while (i < left.length) {
        result.push(left[i]);
        i++;
    }
    while (j < right.length) {
        result.push(right[j]);
        j++;
    }
    return result;
}
module.exports = { mergeSort };
Final Reflection
This exercise showed me that AI can help identify bugs quickly,
but verification is still necessary.
The three verification strategies helped me:
confirm the correctness of the fix
understand the algorithm more deeply
develop better debugging habits
I also learned that:
variable naming matters
loop conditions must always change
testing small examples helps reveal hidden bugs
AI should support learning, not replace understanding