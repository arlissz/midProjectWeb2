Before you start working on your mid project, watch the videos below: 

Learning MockAPI: https://drive.google.com/file/d/1J4sHjv4W5GKrbGizdqWOJFTR5X4Xpeut/view?usp=drivesdk
Using Axios to retrieve data from MockAPI: https://drive.google.com/file/d/1ic7h-O37SE8gXmvnT9CqTe2NAoqjD0wj/view?usp=drivesdk
Create - Update - Delete using Axios VueJs and MockAPI as back-end: https://drive.google.com/file/d/1JGseKTH99GwCOggm0Dh4HJioq5qq-vLt/view?usp=drivesdk
MID Project Instructions: https://drive.google.com/file/d/1zjivHPUXjBQ4q7oVgbf0MfVpkmT3QPhI/view?usp=drivesdk
In this mid project you are required to realize your proposed application in term front-end design. Below are the requirements that you need to complete for your mid term project:

Basic requirements:

The application must be first approved by the lecturer
The application must consist at least (minimum) of 5 tables or resources in the MockAPI (use multiple accounts for creating more resources)
You must complete the assignment using HTML, CSS, (Tailwind v3),  JS (VueJS version 3) based on what we've discussed on our class 
Detailed requirements:

For each function (table or resources), must have: 

An HTML file that CRUD function for each of your table (resource). So if you have five resources you need to have at least 5 files.
Menu in top navbar must consists of links for each resource (table) that you created
One of the file needs to be named index.html (this is the first file that appears when your application is accessed)
Except the index.html other files can be named freely (but remember the links on the navbar should work)
Bonus assignment (10% added point)

Customize your form to take into consideration the resource (table)'s datatype. You can Use input text, radio, textarea, or select tag accordingly. 
Submission Instruction:

Upload all your files used in your project in a .zip file here



<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Learn VueJS with Axios for REST API</title>
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
</head>
<body>
    <div id="app">
        <table>
            <tr>
                <th>ID</th>
                <th>Username</th>
                <th>Firstname</th>
                <th>Lastname</th>
                <th>Email</th>
                <th>Avatar</th>
                <th>Password</th>
                <th>Action</th>
            </tr>
            <tr v-for="user in users">
                <td>{{ user.id }}</td>
                <td>{{ user.username }}</td>
                <td>{{ user.firstname }}</td>
                <td>{{ user.lastname }}</td>
                <td>{{ user.email }}</td>
                <td><img v-bind:src="user.avatar"></td>
                <td>{{ user.password }}</td>
                <td>
                    <button @click="getEditUser(user)">Edit</button>
                    <button @click="deleteUser(user)">Delete</button>
                </td>
            </tr>
        </table>
        <button @click="getUsers">Get Users</button>

        <br><br>
        
        <form @submit.prevent="">
            <h3>Form User</h3>
            <label>Username: </label>
            <input type="text" v-model="user.username"><br>
            <label>Firstname: </label><input type="text" v-model="user.firstname"><br>
            <label>Lastname: </label><input type="text" v-model="user.lastname"><br>
            <label>Email: </label><input type="text" v-model="user.email"><br>
            <label>Avatar: </label><input type="text" v-model="user.avatar"><br>
            <label>Password: </label><input type="text" v-model="user.password"><br>
            <button v-if="!isEdit" @click="addUser">Add User</button>
            <button v-if="isEdit" @click="updateUser">Update User</button>
        </form>
    </div> 
    <script>
        const { createApp } = Vue

        createApp({
            data() {
                return {
                    users: [],
                    user: {
                     username: '',
                     firstname: '',
                     lastname: '',
                     email: '',
                     avatar: '', 
                     password: ''
                },
                isEdit: false,
                userIdSelected: null
            }
        },
            methods: {
                getUsers() {
                    axios
                        .get('https://65e70c0b53d564627a8dbbed.mockapi.io/users')
                        .then((response) => {
                            this.users = response.data
                        })
                },
                addUser() {
                    axios
                        .post("https://65e70c0b53d564627a8dbbed.mockapi.io/users", {
                            username: this.user.username,
                            firstname: this.user.firstname,
                            lastname: this.user.lastname,
                            email: this.user.email,
                            avatar: this.user.avatar,
                            password: this.user.password,
                        })
                        .then((response) => {
                            this.getUsers();
                            alert("New user has been saved");
                            console.log(response);
                            this.resetForm();
                        })
                        .catch((eror) => {
                            console.log(eror);
                        });
                },
                getEditUser(user) {
                    this.isEdit = true
                    this.userIdSelected = user.id
                    this.user.username = user.username
                    this.user.firstname = user.firstname
                    this.user.lastname = user.lastname
                    this.user.email = user.email
                    this.user.avatar = user.avatar
                    this.user.password = user.password
                },
                updateUser() {
                    axios
                    .put(
                        `https://65e70c0b53d564627a8dbbed.mockapi.io/users/${this.userIdSelected}`,
                        {
                            username: this.user.username,
                            firstname: this.user.firstname,
                            lastname: this.user.lastname,
                            email: this.user.email,
                            avatar: this.user.avatar,
                            password: this.user.password,
                        }
                    )
                    .then((response) => {
                        this.getUsers();
                        alert("User has been updated");
                        console.log(response);
                        this.isEdit = false;
                        this.resetForm();
                    })
                    .catch((error) => {
                        console.log(eror);
                    });
                },
                deleteUser(user) {
                    if (confirm("Are you user want to delete this user?")) {
                    axios
                        .delete(
                            `https://65e70c0b53d564627a8dbbed.mockapi.io/users/${user.id}`
                        )
                        .then ((res) => {
                            //handle succes
                            alert("User has been deleted");
                            this.getUsers();
                            this.resetForm();
                        })
                        .catch((err) => {
                            //handle error
                            console.log(err);
                        }); 
                    }
                },
                resetForm() {
                    this.user.username = ''
                    this.user.firstname = ''
                    this.user.lastname = ''
                    this.user.email = ''
                    this.user.avatar = ''
                    this.user.password = ''
                }
            } 
        }).mount('#app')
    </script>
</body>
</html>
