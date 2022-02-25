<script setup>
import { ref, computed, reactive } from 'vue'
const logoSize = 'width:242px;';

const courseNameItems = reactive([
    { classID: '1', courseName: '', grade: 3, credit: 2 },
    { classID: '2', courseName: '', grade: 4, credit: 3 },
    { classID: '3', courseName: '', grade: 3, credit: 2 },
    { classID: '4', courseName: '', grade: 3, credit: 1 },
    { classID: '5', courseName: '', grade: 4, credit: 3 }
    //คิดว่าไม่จำเป็นต้องเก็บข้อมูล obj อย่างนี้ก็ได้นะ แค่ใส่เป็นเลข 0 ก็ได้ แทน index ลำดับๆไป
]);
const gradeSelection = ref([]);
const creditSelection = ref([]);

// สูตรคำนวณเกรด สมมติมี 5 วิชา 
// เกรดที่ได้ A(4) A(4) B(3) A(4) A(4)
// หน่วยกิต 2 3 3 2 2
// (เกรดที่ได้ของวิชา1 x หน่วยกิตของวิชา1)+(เกรดที่ได้ของวิชา2 x หน่วยกิตของวิชา2)+...(เกรดที่ได้ของวิชา5 x หน่วยกิตของวิชา5)
// หารด้วยผลรวมของหน่วยกิจทั้งหมด
// (4x2)+(4x3)+(3x3)+(4x2)+(4x2) / (12)
// 45/12 = 3.75
const gradeCalculation = computed(() => {
    let total = undefined;
    let sumCredit = undefined;
    let sumTotal = undefined;

    if (gradeSelection.value.length > 0) {
        console.log(`มีการกรอกข้อมูลเกรดแล้ว, ${gradeSelection.value}`);
        total = gradeSelection.value.map(
            (grade, credit) => grade * creditSelection.value[credit]
        ).reduce((p, c) => p + c, 0);
        sumCredit = creditSelection.value.reduce((prev, curr) => prev + curr, 0);
        //มัน return ผลลัพธ์ของเกรด * หน่วยกิตออกมาเป็น object
    }
    //console.log(typeof (total));
    // let sumTotal = total.reduce((prev,curr) => prev+curr,0);
    console.log("ผลลัพธ์ของเกรด x หน่วนกิต = " + total);
    console.log("ผลรวมของหน่วยกิตทั้งหมด = " + sumCredit);
    return (total / sumCredit).toFixed(2)
})

const findSumCredit = computed(() => {
    let sumCredit = undefined;
    sumCredit = creditSelection.value.reduce((prev, curr) => prev + curr, 0);
    return sumCredit;
})


// test----------------
const test = ref(() => {
    courseNameItems.splice(5,0,0)
})


</script>
 
<template>
    <div>
        <nav class="container flex justify-around bg-white shadow-md">
            <div class="flex items-center">
                <img src="./assets/ABC WishYourGrade LOGO.png" alt="LOGO" :style="logoSize" />
            </div>
            <!-- left header section -->
            <div class="items-center hidden space-x-8 lg:flex">
                <a href>Home</a>
                <a href>About Us</a>
                <a href>Contact Us</a>
            </div>
            <!-- right header section -->
        </nav>
        <!-- <div class="content-center"> -->
        <!-- <div>  
             <span>  </span>
        </div>-->
        <div class="grid-container">
            <div class="pborder">
                <p>check in array</p>

                <table class="table-fixed" style="width:50%">
                    <thead>
                        <tr>
                            <th>courseID</th>
                            <th>Course</th>
                            <th>Grade</th>
                            <th>Credit</th>
                        </tr>
                    </thead>
                    <tbody>
                        <!-- <tr v-for="course in courseNameItem" :key="course.courseID">
                            <td>{{ course.courseID }}</td>
                            <td>{{ course.courseName }}</td>
                            <td>{{ course.grade }}</td>
                            <td>{{ course.credit }}</td> 
                            
                            //ใช้เพื่อแสดงข้อมูลจาก array
                        -->

                        <tr
                            v-for="(item, index) in courseNameItems"
                            :key="index"
                            class="text-center"
                        >
                            <td>{{ index + 1 }}</td>
                            <td>
                                <input
                                    type="text"
                                    class="border"
                                    placeholder="e.g INT101 Fundamental Programming"
                                />
                            </td>
                            <td>
                                <select v-model="gradeSelection[index]" class="border bg-white">
                                    <option value>Please select your grade</option>
                                    <option value="4">4(A)</option>
                                    <option value="3.5">3.5(B+)</option>
                                    <option value="3">3(B)</option>
                                    <option value="2.5">2.5(C+)</option>
                                    <option value="2">2(C)</option>
                                    <option value="2">1.5(D+)</option>
                                    <option value="1">1(D)</option>
                                    <option value="0">0(F)</option>
                                </select>
                            </td>
                            <td>
                                <input
                                    type="number"
                                    class="border"
                                    placeholder="Enter course's credit"
                                    min="0.5"
                                    step="0.5"
                                    v-model="creditSelection[index]"
                                />
                            </td>
                        </tr>
                    </tbody>
                </table>
                <div>
                    Your Semester GPA is : {{ gradeCalculation }}
                    <br />
                    <!-- Total Credit is :{{ creditSelection }}
                    <br />
                    ผลรวมของเกรด * หน่วยกิต : {{ gradeCalculation }}
                    <br />
                    ผลรวมของหน่วยกิตทั้งหมด : {{ findSumCredit }}-->
                    <br><button v-on:click="test"  class="py-2 px-4 rounded-lg bg-blue-500 text-white font-bold">
                        Add Row
                        </button>
                </div>
            </div>
        </div>
    </div>
    <!-- </div> -->
</template>
 
<style>
.pborder {
    border: 2px solid rgb(19, 18, 18);
    border-radius: 9px;
    padding: 3px;
}
</style>

