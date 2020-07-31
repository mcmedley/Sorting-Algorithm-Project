<template style="background-color: black">
    <div class="home" :style="myStyle">
        <div id="outer">
            <button v-on:click="randoNum(5, 1000)">Generate New Array</button>
            <button v-on:click="quicksort(numbers, 0, numbers.length - 1)">Quick Sort</button>
            <button v-on:click="mergesort(numbers)">Merge Sort</button>
            <button v-on:click="insertionsort(numbers)">Insertion Sort</button>
            <button v-on:click="heapsort(numbers)">Heap Sort</button>
            <button v-on:click="bubblesort(numbers)">Bubble Sort</button>
        </div>
        <svg width="12" height="1000" v-for="number in numbers" v-bind:key="number">
            <rect width="12" :height="(number.value)/2" style="fill:#4cff00;stroke-width:3;stroke:rgb(0,0,0)" />
        </svg>
    </div>
</template>

<script>
    export default {
        name: 'Home',
        data: function () {
            return {
                numbers: [
                    { value: 5 }
                ]
            };
        },
        methods: {
            bColor: function (color) {
                document.template.style.background = color;
            },
            randoNum: function (min, max) {
                this.numbers = [];
                for (var i = 0; i < 100; i++) {
                    this.numbers.push({ value: Math.floor(Math.random() * (max - min) + min) });
                }
            },
            swap: function (items, leftIndex, rightIndex) {
                var temp = items[leftIndex].value;
                items[leftIndex].value = items[rightIndex].value;
                items[rightIndex].value = temp;
            },
            partition: function (items, left, right) {
                var pivot = items[Math.floor((right + left) / 2)].value //middle element
                var i = left //left pointer
                var j = right; //right pointer
                while (i <= j) {
                    while (items[i].value < pivot) {
                        i++;
                    }
                    while (items[j].value > pivot) {
                        j--;
                    }
                    if (i <= j) {
                        setTimeout(() => this.swap(items, i, j), 250);
                        i++;
                        j--;
                    }
                }
                return i;
            },
            quicksort: function (items, left, right) {
                var index;
                if (items.length > 1) {
                    index = this.partition(items, left, right); //index returned from partition
                    if (left < index - 1) { //more elements on the left side of the pivot
                        this.quicksort(items, left, index - 1);
                    }
                    if (index < right) { //more elements on the right side of the pivot
                        this.quicksort(items, index, right);
                    }
                }
                return items;
            },

            merge: function (left, right) {
                let resultArray = [], leftIndex = 0, rightIndex = 0;

                // We will concatenate values into the resultArray in order
                while (leftIndex < left.length && rightIndex < right.length) {
                    if (left[leftIndex].value < right[rightIndex].value) {
                        resultArray.push({ value: left[leftIndex].value });
                        leftIndex++; // move left array cursor
                    } else {
                        resultArray.push(right[rightIndex]);
                        rightIndex++; // move right array cursor
                    }
                }

                // We need to concat here because there will be one element remaining
                // from either left OR the right
                return resultArray.concat(left.slice(leftIndex)).concat(right.slice(rightIndex));
            },
            mergesort: function (arr) {
                if (arr.length < 2) {
                    // return once we hit an array with a single item
                    return arr;
                }

                const middle = Math.floor(arr.length / 2); // get the middle item of the array rounded down
                const left = arr.slice(0, middle); // items on the left side
                const right = arr.slice(middle); // items on the right side

                return this.merge(this.mergesort(left), this.mergesort(right));
            },

            insertionsort: function (array) {
                for (let i = 1; i < array.length; i++) {
                    let key = array[i].value;
                    let j = i - 1;
                    while (j >= 0 && array[j].value > key) {
                        array[j + 1].value = array[j].value;
                        j = j - 1;
                    }
                    array[j + 1].value = key;
                }
                return array;
            },

            heaproot: function (input, i, length) {
                var left = 2 * i + 1;
                var right = 2 * i + 2;
                var max = i;

                if (left < length && input[left].value > input[max].value) {
                    max = left;
                }

                if (right < length && input[right].value > input[max].value) {
                    max = right;
                }

                if (max != i) {
                    this.swap(input, i, max);
                    this.heaproot(input, max, length);
                }

            },
            heapsort: function (input) {
                var array_length = input.length;
                for (var i = Math.floor(array_length / 2); i >= 0; i--) {
                    this.heaproot(input, i, array_length);
                }

                for (i = input.length - 1; i > 0; i--) {
                    this.swap(input, 0, i);
                    array_length--;
                    this.heaproot(input, 0, array_length);
                }

            },
            bubblesort: function (arr) {
                var test = true;
                while (test) {
                    test = false;
                    for (var j = 0; j < arr.length-1; j++) {
                        if (arr[j].value > arr[j + 1].value) {
                            test = true;
                        }
                    }
                }
                return arr;
            }
        },
    }
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
    .outer button {
        background-color: #4CAF50; /* Green background */
        border: 1px solid green; /* Green border */
        color: white; /* White text */
        padding: 10px 24px; /* Some padding */
        cursor: pointer; /* Pointer/hand icon */
        float: right; /* Float the buttons side by side */
    }

        .outer button:not(:last-child) {
            border-right: none; /* Prevent double borders */
        }

    /* Clear floats (clearfix hack) */
    .outer:after {
        content: "";
        clear: both;
        display: table;
    }

    /* Add a background color on hover */
    .outer button:hover {
        background-color: #3e8e41;
    }

    template {
        background-color: red;
    }
</style>

