<template>
    <div id="container">
        <div id="form">
            <!-- User Email  -->
            <div class="field">
                <label for="user-email" class="label-field"
                    :class="{ 'red-font-color': userEmailEmptyError }">EMAIL:</label>
                <input type="text" id="user-email" name="user-email" class="input-field" v-model="userEmail"
                    :class="{ 'red-border-bottom': userEmailEmptyError }">
                <p class="error" v-if="userEmailEmptyError"><i class="fa-solid fa-circle-exclamation"></i> Enter your
                    email</p>
            </div>
            <!-- User Password  -->
            <div class="field">
                <label for="user-password" class="label-field"
                    :class="{ 'red-font-color': userPasswordEmptyError || userPasswordValidationError }">PASSWORD:</label>
                <input type="password" id="user-password" name="user-password" class="input-field"
                    v-model="userPassword"
                    :class="{ 'red-border-bottom': userPasswordEmptyError || userPasswordValidationError }">
                <p class="error" v-if="userPasswordValidationError"><i class="fa-solid fa-circle-exclamation"></i>
                    Password length should be 8 char, 1 special char, 1 number, 1 uppercase, and 1 lowercase</p>
                <p class="error" v-if="userPasswordEmptyError"><i class="fa-solid fa-circle-exclamation"></i> Enter your
                    password</p>
            </div>
            <!-- User Role  -->
            <div class="field">
                <label for="user-role" class="label-field"
                    :class="{ 'red-font-color': userRoleEmptyError }">ROLE:</label>
                <select id="user-role" v-model="userRole">
                    <option value="" disabled>Select role</option>
                    <option value="Web Developer">Web Developer</option>
                    <option value="Software Engg">Software Engg</option>
                    <option value="Tester">Tester</option>
                </select>
                <p class="error" v-if="userRoleEmptyError"><i class="fa-solid fa-circle-exclamation"></i> Select your
                    role</p>
            </div>
            <!-- User Skills  -->
            <div class="field">
                <label for="skills" class="label-field"
                    :class="{ 'red-font-color': userSkillEmptyError }">SKILLS:</label>
                <input type="text" v-model="skillInput" @keyup.enter="addSkill" class="input-field"
                    :class="{ 'red-border-bottom': userSkillEmptyError }">
                <div id="skill-list-div">
                    <span v-for="(skill, index) in skillsList" class="skill-box" :id="index" :key="index">
                        {{ skill }}
                        <button class="skill-delete-button" :key="index" :id="index"
                            @click="skillDeleteHandler(index)"><i class="fa-solid fa-xmark"></i></button>
                    </span>
                </div>
                <p class="error" v-if="userSkillEmptyError"><i class="fa-solid fa-circle-exclamation"></i> Add your
                    skill/skills</p>
            </div>
            <!-- Terms And Conditions checkbox  -->
            <input type="checkbox" name="terms-and-conditions" id="terms-and-conditions" v-model="termsAndConditions">
            <label for="terms-and-conditions" id="terms-and-conditions-label">ACCEPT TERMS AND CONDITIONS</label>
            <p class="error" v-if="termsAndConditionsUncheckedError"><i class="fa-solid fa-circle-exclamation"></i>
                Check the terms and conditions field</p>
            <!-- "Create an Account" button  -->
            <button @click="createAccount" id="create-account-btn">Create an Account</button>

        </div>
        <div v-if="formValidated">
            <p>Email:{{ userEmail }}</p>
            <p>Password: {{ userPassword }}</p>
            <p>Role: {{ userRole }}</p>
            <p>Skills: {{ skillsList }}</p>
            <p>Terms and Conditions: {{ termsAndConditions }}</p>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            userEmail: "",
            userPassword: "",
            userRole: "",
            termsAndConditions: false,
            regularExpression: /^(?=.*?[A-Z])(?=(.*[a-z]){1,})(?=(.*[\d]){1,})(?=(.*[\W]){1,})(?!.*\s).{8,}$/,
            formValidated: "",
            skillsList: [],
            skillInput: "",
            userEmailEmptyError: false,
            userPasswordEmptyError: false,
            userPasswordValidationError: false,
            userRoleEmptyError: false,
            userSkillEmptyError: false,
            termsAndConditionsUncheckedError: false,
        }
    },
    methods: {
        // function for "create an account" button: validation and submit
        createAccount() {
            this.formValidated = true;
            this.userEmailEmptyError = false;
            this.userPasswordEmptyError = false;
            this.userPasswordValidationError = false;
            this.userRoleEmptyError = false;
            this.userSkillEmptyError = false;
            this.termsAndConditionsUncheckedError = false;

            if (this.userEmail == "") {
                this.userEmailEmptyError = true;
                this.formValidated = false;
            }
            if (this.userPassword == "") {
                this.userPasswordEmptyError = true;
                this.formValidated = false;
            }
            if (this.userRole == "") {
                this.userRoleEmptyError = true;
                this.formValidated = false;
            }
            if (this.skillsList.length == 0) {
                this.userSkillEmptyError = true;
                this.formValidated = false;
            }
            if (this.userEmail != "" && this.userPassword != "" && this.userRole != "" && this.skillsList.length != 0) {
                if (this.termsAndConditions == false) {
                    this.termsAndConditionsUncheckedError = true;
                    this.formValidated = false;
                }
            }
            if (this.userPassword != "") {
                if (!this.regularExpression.test(this.userPassword)) {
                    this.userPasswordValidationError = true;
                    this.formValidated = false
                }
            }
        },
        // funtion when enter or comma key is pressed in skills input to add skill
        addSkill() {
            if (this.skillInput != "") {
                if (this.skillsList.length == 0) {
                    this.userSkillEmptyError = false;
                }
                this.skillsList.push(this.skillInput);
                this.skillInput = "";
            }
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
    margin-top: 15px;
    background-color: rgb(228, 228, 228);
    padding: 5px 2px;
}

#user-role:focus {
    border: none;
    outline: none;
}

#user-role option {
    background-color: rgb(236, 236, 236);
}

#terms-and-conditions-label {
    margin-left: 10px;
}

.error {
    color: tomato;
    margin: 5px;
    font-size: .8em;
}

.red-border-bottom {
    border-bottom: 1px solid tomato !important;
}

.red-font-color {
    color: tomato !important;
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
    font-size: .94em;
    letter-spacing: 1px;
}

.skill-delete-button {
    background-color: transparent;
    border: none;
}

.skill-delete-button:hover {
    font-size: .99em;
}

#skill-list-div {
    margin-top: 20px
}
</style>
