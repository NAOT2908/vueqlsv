<template>
    <form>
        <label for="search" class="mb-2 text-sm font-medium text-gray-900 sr-only dark:text-white">Search</label>
        <div class="relative">
            <div class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none">
                <svg aria-hidden="true" class="w-5 h-5 text-gray-500 dark:text-gray-400" fill="none" stroke="currentColor"
                    viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                        d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path>
                </svg>
            </div>
            <input type="search" id="search" v-model="searchip"
                class="block w-full p-4 pl-10 text-sm text-gray-900 border border-gray-300 rounded-lg bg-gray-50 focus:ring-blue-500 focus:border-blue-500 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                placeholder="Search" required>
            <button type="button" @click="onSeacrh()"
                class="text-white absolute right-2.5 bottom-2.5 bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-4 py-2 dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800">Search</button>
        </div>
    </form>
    <div class="formsv">
        <div class="forminput"> 
            <form class="form1 bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4  ">
                <div class="mb-4 px-2 w-full">
                    <label class="block mb-1 text-sm " for="input1">Mã sinh viên:</label>
                    <input id="input1"
                        class="w-full border px-4 py-2 rounded-full focus:border-blue-500 focus:shadow-outline outline-none outline-none"
                        type="text" placeholder="Mã sinh viên..." v-model="Id" />


                    <label class="block mb-1 mt-4 text-sm" for="input2">Tên sinh viên:</label>
                    <input id="input2"
                        class="w-full border px-4 py-2 rounded-full focus:border-blue-500 focus:shadow-outline outline-none outline-none"
                        type="text" v-model="Name" placeholder="Tên sinh viên..." />

                    <label class="block mb-1 mt-4 text-sm" for="inputns">Ngày sinh:</label>
                    <input id="inputns"
                        class="w-full border px-4 py-2 rounded-full focus:border-blue-500 focus:shadow-outline outline-none outline-none"
                        type="date" v-model="Datetime" />

                    <label class="block mb-1 mt-4 text-sm">Giới tính:</label>

                    <div class="flex items-center mb-4">
                        <input id="male" type="radio" value="Nam" name="default-radio" v-model="gender"
                            class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
                        <label for="male" class="ml-2 text-sm font-medium text-gray-900 dark:text-gray-300">Nam</label>
                    </div>
                    <div class="flex items-center">
                        <input id="female" type="radio" value="Nữ" name="default-radio" v-model="gender"
                            class="w-4 h-4 text-blue-600 bg-gray-100 border-gray-300 focus:ring-blue-500 dark:focus:ring-blue-600 dark:ring-offset-gray-800 focus:ring-2 dark:bg-gray-700 dark:border-gray-600">
                        <label for="female" class="ml-2 text-sm font-medium text-gray-900 dark:text-gray-300">Nữ</label>
                    </div>
                </div>

                <div class="mb-4 px-2 w-full">
                    <label class="block mb-1 text-sm" for="select-item">Khoa:</label>

                    <div class="relative">
                        <select
                            class="w-full border px-4 pr-8 py-2 rounded focus:border-blue-500 focus:shadow-outline outline-none appearance-none bg-orange-100"
                            id="select-item" v-model="Faculty">
                            <option value=""></option>
                            <option value="Công nghệ thông tin">Công nghệ thông tin</option>
                            <option value="Kinh tế">Kinh tế</option>
                            <option value="Điện tử viễn thông">Điện tử viễn thông</option>
                        </select>
                        <div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700">
                            <svg class="fill-current h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
                                <path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z" />
                            </svg>
                        </div>
                    </div>
                </div>
                <div class="btn">
                    <button type="button" @click="Addstudent()"> Thêm mới </button>
                    <button type="button" @click="Updatestudent()"> Cập nhật </button>
                </div>
            </form>
        </div>
        <div class="">
            <StudentLists @searchStudent="onSeacrh" @editStudent="onEdit" ref="child" />
        </div>
    </div>
</template>

<script>
import StudentLists from './StudentLists.vue';
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';

export default {

    components: {
        StudentLists,

    },
    data() {
        return {
            Id: "",
            Name: "",
            gender: "",
            Datetime: "",
            Faculty: "",
            searchip: "",

        }
    },
    methods: {
        Addstudent() {
            if (this.Id && this.gender && this.Datetime && this.Name && this.Faculty) {
                const isCheckID = this.$refs.child.students.find(
                    (student) => student.id === this.Id);
                if (isCheckID) {
                    toast.warning('ID đã có', {
                        autoClose: 1000,
                    });
                } else {
                    const newStudent = {
                        id: this.Id,
                        name: this.Name,
                        date: this.Datetime,
                        gender: this.gender,
                        faculty: this.Faculty,
                    };
                    console.log(newStudent)
                    setTimeout(() => {
                        this.$refs.child.addTable(newStudent);
                    }, 1500);
                    this.Id = "";
                    this.Name = "";
                    this.Datetime = "";
                    this.gender = "";
                    this.Faculty = "";
                    //   localStorage.setItem('data', JSON.stringify([newStudent]))
                    toast.success("Thêm thành công", {
                        autoClose: 1000
                    })

                }
            } else {
                toast.error('Chưa nhập đủ thông tin',
                    {
                        autoClose: 1000
                    })

            }
        },
        Updatestudent() {
            const updateStudent = {
                id: this.Id,
                name: this.Name,
                date: this.Datetime,
                gender: this.gender,
                faculty: this.Faculty,
            };
            console.log(updateStudent)
            this.$refs.child.updateTable(updateStudent);
            this.Id = "";
            this.Name = "";
            this.Datetime = "";
            this.gender = "";
            this.Faculty = "";


        },
        onEdit(student) {
            this.Id = student.id;
            this.gender = student.gender;
            this.Name = student.name;
            this.Datetime = student.date;
            this.Faculty = student.faculty;

        },
        onSeacrh() {

            this.$refs.child.searchStudent(this.searchip);
        }
    },
    // mounted() {
    //     console.log(this.$route.params.id);
    //     this.Id = this.$route.params.id;
    //     const liststudent = localStorage.getItem('students') ? JSON.parse(localStorage.getItem('students')).find((item) => item.id === this.$route.params.id) : []
    //     console.log(liststudent);
    //     this.Name = liststudent.name;
    //     this.Datetime = liststudent.date;
    //     this.gender = liststudent.gender;
    //     this.Faculty = liststudent.faculty;


    // },

}

</script>

<style scoped>
.formsv {
    color: red;
    font-size: medium;
    display: flex;


}

.formsv>.forminput {
    width: 500px;
    border-right: 1px solid #ccc;
    border-radius: 1px;
}

.form1>.btn button {
    display: flex;
    justify-content: center;
    box-shadow: 0 0 5px cyan,
        0 0 25px cyan;
    border: 1px solid cyan;
    margin: 5px;
    text-align: center;
    padding: 2px;
    border-radius: 5px;
    background-color: cyan;
    color: black;

}

.form1>.btn button:hover {
    box-shadow: 0 0 5px cyan,
        0 0 25px cyan,
        0 0 50px cyan,
        0 0 100px cyan,
        0 0 200px cyan;
}

.btn {
    display: flex;
    justify-content: center;
}
</style>