<template>
    <!-- <StudentListItem v-for="student in students" :key="student.id" :student="student" />
    <StudentListItem v-if="newStudent" :student="newStudent" />  -->
    <div>
        <table class="w-full text-sm text-left text-gray-500 dark:text-gray-400">
            <thead class="text-xs text-gray-700 uppercase bg-gray-50 dark:bg-gray-700 dark:text-gray-400">
                <tr>
                    <th scope="col" class="p-4">
                        <div class="flex items-center">
                            <button @click="delAll()"
                                class="text-white bg-red-700 hover:bg-red-500 focus:outline-none focus:ring-4 focus:ring-red-300 font-medium rounded-full text-sm px-4 py-2 text-center mr-1 mb-1 dark:bg-red-600 dark:hover:bg-red-700 dark:focus:ring-red-900">Xoá</button>
                        </div>
                    </th>
                    <th scope="col" class="px-6 py-3 text-center ">
                        <div class=" items-center ">
                            Mã sinh viên
                            <button @click="sortTable()"><svg xmlns="http://www.w3.org/2000/svg" class="w-3 h-3 ml-1"
                                    aria-hidden="true" fill="currentColor" viewBox="0 0 320 512">
                                    <path
                                        d="M27.66 224h264.7c24.6 0 36.89-29.78 19.54-47.12l-132.3-136.8c-5.406-5.406-12.47-8.107-19.53-8.107c-7.055 0-14.09 2.701-19.45 8.107L8.119 176.9C-9.229 194.2 3.055 224 27.66 224zM292.3 288H27.66c-24.6 0-36.89 29.77-19.54 47.12l132.5 136.8C145.9 477.3 152.1 480 160 480c7.053 0 14.12-2.703 19.53-8.109l132.3-136.8C329.2 317.8 316.9 288 292.3 288z" />
                                </svg>
                            </button>
                        </div>
                    </th>
                    <th scope="col" class="px-6 py-3 text-center">
                        Tên sinh viên
                    </th>
                    <th scope="col" class="px-6 py-3 text-center">
                        Ngày sinh
                    </th>
                    <th scope="col" class="px-6 py-3 text-center">
                        Giới tính
                    </th>
                    <th scope="col" class="px-6 py-3 text-center">
                        Khoa
                    </th>
                    <th scope="col" class="px-6 py-3 text-center">
                       
                    </th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="student in students" :key="student.id"
                    class="bg-white border-b dark:bg-gray-800 dark:border-gray-700 hover:bg-gray-50 dark:hover:bg-gray-600">
                    <td class="w-4 p-4">
                        <div class="items-center">
                            <input id="checkbox-table-search-1" type="checkbox" v-model="selectStudents" :value="student"
                                class=" ml-5 w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 rounded focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 dark:focus:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
                            <label for="checkbox-table-search-1" class="sr-only">checkbox</label>
                        </div>
                    </td>
                    <th scope="row"
                        class="px-6 py-4 text-gray-900 whitespace-nowrap dark:text-white text-center ">
                        {{ student.id }}
                    </th>
                    <td class="px-6 py-4 text-gray-900 text-center">
                        {{ student.name }}
                    </td>
                    <td class="px-6 py-4 text-gray-900 text-center">
                        {{ student.date }}
                    </td>
                    <td class="px-6 py-4 text-gray-900 text-center">
                        {{ student.gender }}
                    </td>
                    <td class="px-6 py-4 text-gray-900 text-center">
                        {{ student.faculty }}
                    </td>
                    <td class="px-6 py-4 pl-10 ">
                        <button type="button" @click="editStudent(student)"
                            class="text-white bg-blue-700 hover:bg-blue-500 focus:outline-none focus:ring-4 focus:ring-blue-300 font-medium rounded-full text-sm px-5 py-2.5 text-center mr-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">

                            <!-- <router-link :to="`/home/${student.id}`">Sửa</router-link> -->
                            Sửa
                        </button>
                        <button type="button" @click="delStudent(student.id)"
                            class=" text-white bg-red-700 hover:bg-red-500 focus:outline-none focus:ring-4 focus:ring-red-300 font-medium rounded-full text-sm px-5 py-2.5 text-center mr-2 mb-2 dark:bg-red-600 dark:hover:bg-red-700 dark:focus:ring-red-900">Xoá</button>
                    </td>
                </tr>

            </tbody>
        </table>
    </div>
</template>

<script>
import { toast } from 'vue3-toastify';


export default {
    props: {

    },
    data() {
        return {
            students: localStorage.getItem("students") ? JSON.parse(localStorage.getItem('students')) : [],
            selectStudents: [],
            clickCount: 0,
            originalStudents: [],

        }
    },
    created() {
        // Lưu bảng sinh viên ban đầu
        this.originalStudents = JSON.parse(JSON.stringify(this.students))
    },
    methods: {
        sortAscending() {
            // Sắp xếp bảng theo thứ tự tăng dần
            this.students.sort((a, b) => a.id - b.id);
            localStorage.setItem("students", JSON.stringify(this.students));
        },
        sortDescending() {
            // Sắp xếp bảng theo thứ tự giảm dần
            this.students.sort((a, b) => b.id - a.id);
            localStorage.setItem("students", JSON.stringify(this.students));
        },
        sortTable() {
            this.clickCount++
            if (this.clickCount % 2 === 0) {
                this.sortDescending()
            } else {
                this.sortAscending()
            }
        },
        addTable(student) {
            this.students.push(student)
            localStorage.setItem('students', JSON.stringify(this.students))
        },
        delStudent(id) {
            const index = this.students.findIndex(student => student.id === id);
            if (index !== -1) {
                this.students.splice(index, 1);
                localStorage.setItem('students', JSON.stringify(this.students));
            }
        },
        editStudent(student) {
            // emit sự kiện 'editstudent' với giá trị student được truyền vào
            this.$emit('editStudent', student);
            this.$route.id
            console.log(student)
        },
        updateTable(updateStudent) {
            const index = this.students.findIndex(student => student.id === updateStudent.id);
            if (index !== -1) {
                this.students.splice(index, 1, updateStudent);
                localStorage.setItem('students', JSON.stringify(this.students));
            }
            console.log(updateStudent)
        },
        delAll() {
            if (this.selectStudents.length === 0) {

                return
            }
            if (toast.success(`Bạn đã xoá ${this.selectStudents.length} sinh viên`)) {
                this.students = this.students.filter((student) => !this.selectStudents.includes(student));
                localStorage.setItem('students', JSON.stringify(this.students));

            }
        },
        searchStudent(searchip) {
            if (searchip) {
                this.students = this.students.filter((student) => {
                    return (
                        student.name.toLowerCase().includes(searchip) ||
                        student.id.toLowerCase().includes(searchip)

                    );
                })
            } else {
                this.students = this.originalStudents;
            }


            console.log(this.students)
            // localStorage.setItem('students', JSON.stringify(this.students))
        },

    },




}


</script>

<style></style>