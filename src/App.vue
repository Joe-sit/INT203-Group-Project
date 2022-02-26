<script setup>
import { ref, computed, reactive } from 'vue'
const logoSize = 'width:242px;';

const courseNameItems = reactive([]);
const gradeSelection = ref([]);
const creditSelection = ref([]);
const changColor =ref(undefined)


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
const addRow = ref(() => {
    courseNameItems.splice(courseNameItems.length,0,'')
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
                <a href >Home</a>
                <a href>About Us</a>
                <a href>Feedback</a>
            </div>
            <!-- right header section -->
        </nav>
        <!-- <div class="content-center"> -->
        <!-- <div>  
             <span>  </span>
        </div>-->
        <div class="grid-container" >
            <div class="container" >
                <!-- <p>check in array</p> -->
                <div class="table">
                <table class="table-fixed" style="margin: auto;">
                    <thead>
                        <tr v-if="courseNameItems.length>0">
                            <th>NO.</th>
                            <th>Course</th>
                            <th>Grade</th>
                            <th>Credit</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr
                            v-for="(item, index) in courseNameItems"
                            :key="index"
                            class="text-center"
                        >
                            <td>{{ index + 1 }}</td>
                            <td>
                                <input v-model="courseNameItems[index]" type="text"
                                    
                                    placeholder="e.g INT101 Fundamental Programming"
                                />
                            </td>
                            <td>
                                <select v-model="gradeSelection[index]" >
                                    <option disabled >Please select your grade</option>
                                    <option value="4">A</option>
                                    <option value="3.5">B+</option>
                                    <option value="3">B</option>
                                    <option value="2.5">C+</option>
                                    <option value="2">C</option>
                                    <option value="2">D+</option>
                                    <option value="1">D</option>
                                    <option value="0">F</option>
                                </select>
                            </td>
                            <td>
                                <input
                                    type="number"
                                    
                                    placeholder="Enter course's credit"
                                    min="0.5"
                                    step="0.5"
                                    v-model="creditSelection[index]"
                                />
                            </td>
                        </tr>
                    </tbody>
                </table>
                </div>
                <div class="result">
                    <p v-if="courseNameItems.length>0">
                    Semester GPA is : 
                    <span v-if="gradeCalculation<2.49" class="square" style="background-color: rgb(238, 78, 78);">{{ gradeCalculation }}</span>
                    <span v-else-if="gradeCalculation<3.49" class="square" style="background-color: rgb(252, 171, 96);">{{ gradeCalculation }}</span>
                    <span v-else-if="gradeCalculation<=4" class="square" style="background-color: rgb(96, 243, 96);">{{ gradeCalculation }}</span>
                    <span v-else class="square" style="color: rgb(255, 0, 0);font-size: 15px;">information is not complete</span>
                    </p>
                    <br>
                    <button v-on:click="addRow"  class="BaddRow">
                        Add Row
                    </button>
                </div>
            </div>
        </div>
    </div>
    <!-- </div> -->
</template>
 
<style>
.table {
    padding: 20px;
    margin: auto;
}
.result{
    margin: auto;
    padding: 20px ;
    text-align: center;
}
.container{
    margin: auto;
    width:50%;
    padding: 20px 10px 10px 10px;

}
body{
    font-size: 20px;
}
.square{
    display: inline-block;
     /* background-color: rgb(73, 209, 73);  */
    width: auto;
    height: auto;
    padding: 8px;
    border-radius:12px ;
     font-family: "Audiowide", sans-serif;
     color: white;
     font-size:  25px;
     font-weight: bold;
}

.BaddRow{
    width: auto;
    height: auto;
    padding: 8px;
    border-radius:12px ;
     font-family: "Audiowide", sans-serif;
     color: rgb(0, 0, 0);
     font-size:  25px;
     font-weight: bold;
     transition: all 0.5s ease;
}
.BaddRow:hover{
    color: rgb(255, 255, 255);
    background-color: rgb(52, 175, 247);
}

</style>

