<template>
<div id="container">
    <div id="form">
        <div class="field">
            <label for="user-email" class="label-field">EMAIL:</label>
            <input type="text" id="user-email" name="user-email" class="input-field" v-model="userEmail">
        </div>
        <div class="field">
            <label for="user-password" class="label-field">PASSWORD:</label>
            <input type="password" id="user-password" name="user-password" class="input-field" v-model="userPassword">
            <p id="password-validation-error">{{passwordValidationError}}</p>
        </div>
        <div class="field">
            <label for="user-role" class="label-field">ROLE:</label>
            <select id="user-role" v-model="userRole">
                <option value="" disabled>Select role</option>
                <option value="Web Developer">Web Developer</option>
                <option value="Software Engg">Software Engg</option>
                <option value="Tester">Tester</option>
            </select>
        </div>
        <div class="field">
            <label for="skills" class="label-field">SKILLS:</label>
            <input type="text" v-model="skillInput" @keyup.enter="addSkill" class="input-field">
            <div id="skill-list-div">
                <span v-for="(skill,index) in skillsList" class="skill-box" :id="index" :key="index">
                    {{skill}}
                    <button class="skill-delete-button" :key="index" :id="index" @click="skillDeleteHandler(index)"><i class="fa-solid fa-xmark"></i></button>
                </span>
            </div>
        </div>
        <input type="checkbox" name="terms-and-conditions" id="terms-and-conditions" v-model="termsAndConditions">
        <label for="terms-and-conditions" id="terms-and-conditions-label">ACCEPT TERMS AND CONDITIONS</label>
        <button @click="createAccount" id="create-account-btn">Create an Account</button>
    </div>
    <div v-if="formValidated">
        <p>Email:{{userEmail}}</p>
        <p>Password: {{userPassword}}</p>
        <p>Role: {{userRole}}</p>
        <p>Skills: {{skillsList}}</p>
    </div>
</div>
</template>

<script>
export default {
    data() {
        return {
            userEmail: "",
            userPassword: "",
            passwordValidationError: "",
            userRole: "",
            termsAndConditions: "false",
            regularExpression: /^(?=.*?[A-Z])(?=(.*[a-z]){1,})(?=(.*[\d]){1,})(?=(.*[\W]){1,})(?!.*\s).{8,}$/,
            formValidated: "",
            skillsList: [],
            skillInput: "",
        }
    },
    methods: {
        // function for "create an account" button: validation and submit
        createAccount() {
            this.formValidated = true;
            this.passwordValidationError = ""
            if (this.userEmail == "" || this.userPassword == "" || this.userRole == "" || this.termsAndConditions == "" || this.skillsList.length == 0) {
                alert("fill all fields")
                this.formValidated = false;
            }
            if (!this.regularExpression.test(this.userPassword)) {
                this.passwordValidationError = "Password length should be 8 char, 1 special char, 1 number, 1 uppercase, and 1 lowercase"
                this.formValidated = false
            }
        },
        // funtion when enter or comma key is pressed in skills input to add skill
        addSkill() {
            this.skillsList.push(this.skillInput);
            this.skillInput = "";
        },
        skillDeleteHandler(index) {
            this.skillsList.splice(index, 1);
        }
    }
}
</script>

<style>
#container {
    width: 60%;
    margin: auto;
    background-color: lightgray;
    padding: 13px;
}

#form {
    background-color: white;
    border-radius: 10px;
    padding: 30px;
}

.field {
    margin-bottom: 30px;
}

.label-field,
.input-field {
    display: block;
    width: 100%;
}

#form label {
    margin-bottom: 20px;
    color: rgb(109, 109, 109);
    font-size: .9em;
    font-weight: bolder;
    letter-spacing: 2px;
}

#form input {
    border: none;
    border-bottom: 1px solid lightgrey;
    padding: 10px 0px;
    font-size: 1em;
    color: rgb(116, 116, 116);
}

#form input:focus {
    outline: none;
}

#user-role {
    width: 100%;
    border: none;
    font-size: 1em;
    color: gray;
    margin-top: 15px
}

#user-role:focus {
    border: none;
    outline: none
}

#terms-and-conditions-label {
    margin-left: 10px;
}

#password-validation-error {
    color: tomato;
    margin: 5px;
    font-size: .8em;
}

#create-account-btn {
    display: block;
    background-color: dodgerblue;
    color: white;
    padding: 10px 25px;
    border-radius: 20px;
    border: none;
    font-size: .95em;
    margin: auto;
    margin-top: 30px;
    cursor: pointer;
}

.skill-box {
    background-color: lightgrey;
    color: rgb(52, 58, 58);
    margin: 0px 7px;
    border: none;
    border-radius: 25px;
    padding: 7px 15px;
    font-size: 1em;
    letter-spacing: 1px;
}

.skill-delete-button {
    background-color: transparent;
    border: none;
}

.skill-delete-button:hover {
    font-size: 1em;
}

#skill-list-div {
    margin-top: 20px
}
</style>
