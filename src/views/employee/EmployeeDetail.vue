<template>
    <div class="popup">
        <div class="popup-content popup-employee">
            <div class="popup__header">
                <div class="popup__header--left">
                    <div class="header-left__title">
                        <h3>{{ title }}</h3>
                    </div>
                    <div class="header-left__input">
                        <div class="is-customer">
                            <input type="checkbox" class="input-checkbox">
                            <label class="pd-l-8">{{ EMPLOYEEDETAIL_TITLE.IS_CUSTOMER }}</label>
                        </div>
                        <div class="is-supplier">
                            <input type="checkbox" class="input-checkbox">
                            <label class="pd-l-8">{{ EMPLOYEEDETAIL_TITLE.IS_SUPPLIER }}</label>
                        </div>
                    </div>
                </div>
                <div class="popup__header--right">
                    <div class="popup__icon--help" :title="TOOLTIP.HELP"></div>
                    <div @click="hideEmployeeDetail" :title="TOOLTIP.CLOSE" class="popup__icon--close">
                    </div>
                </div>
            </div>
            <div class="popup__body">
                <div class="half-block-horizontal flex pd-b-14">
                    <div class="block-vertical">
                        <div class="flex pd-b-14">
                            <div class="text-field popup-item-160">
                                <div ref="firstFocus" tabindex="1"></div>
                                <label class="pd-b-8 popup-lable">{{ EMPLOYEEDETAIL_TITLE.CODE }}
                                    <h3 class="pd-l-4">*</h3>
                                </label>
                                <input tabindex="2" class="input" type="text" ref="inputEmployeeCode"
                                    :class="{ 'border-red': isEmployeeCodeNull }" v-model="employee.EmployeeCode"
                                    @blur="validateEmployeeCode" :title="titleEmployeeCode">
                            </div>
                            <div class="text-field popup-item-260 pd-l-8">
                                <label class="pd-b-8 popup-lable">{{ EMPLOYEEDETAIL_TITLE.NAME }}
                                    <h3 class="pd-l-4">*</h3>
                                </label>
                                <input tabindex="3" class="input" type="text" ref="inputEmployeeName"
                                    :class="{ 'border-red': isEmployeeNameNull }" v-model="employee.EmployeeName"
                                    @blur="validateEmployeeName" :title="titleEmployeeName">
                            </div>
                        </div>
                        <div class="text-field popup-item-428 pd-b-14">
                            <label class="pd-b-8 popup-lable">{{ EMPLOYEEDETAIL_TITLE.DEPARTMENT }}
                                <h3 class="pd-l-4">*</h3>
                            </label>
                            <select tabindex="6" class="select" name="departments" v-model="employee.DepartmentID"
                                :class="{ 'border-red': isDepartmentNull }" @blur="validateDepartment"
                                :title="titleDepartment">
                                <option v-for="(item, index) in departments" :key="index" :value="item.DepartmentID"
                                    :selected="item.DepartmentID == employee.DepartmentID">
                                    {{ item.DepartmentName }}
                                </option>
                            </select>
                        </div>
                        <div class="text-field popup-item-428">
                            <label class="pd-b-8 popup-lable">{{ EMPLOYEEDETAIL_TITLE.JOBPOSITION }}</label>
                            <input tabindex="9" class="input" type="text" v-model="employee.JobPosition">
                        </div>
                    </div>
                    <div class="block-vertical">
                        <div class="text-field popup-item-160 pd-b-14">
                            <label class="pd-b-8 popup-lable">{{ EMPLOYEEDETAIL_TITLE.DATEOFBIRTH }}</label>
                            <input tabindex="4" class="input" type="date" v-model="employee.DateOfBirth">
                        </div>
                        <div class="text-field pd-l-16 pd-b-14">
                            <label class="pd-b-8 popup-lable">{{ EMPLOYEEDETAIL_TITLE.GENDER }}</label>
                            <div class="radio-gender">
                                <div class="detail-gender">
                                    <input tabindex="5" id="male" class="radio-box" name="gender" type="radio"
                                        :value="GENDER.MALE.VALUE" v-model="employee.Gender">
                                    <label class="pd-l-8" for="male">{{ EMPLOYEEDETAIL_TITLE.MALE }}</label>
                                </div>
                                <div class="detail-gender pd-l-16">
                                    <input id="female" class="radio-box" name="gender" type="radio"
                                        :value="GENDER.FEMALE.VALUE" v-model="employee.Gender">
                                    <label class="pd-l-8" for="female">{{ EMPLOYEEDETAIL_TITLE.FEMALE }}</label>
                                </div>
                                <div class="detail-gender pd-l-16">
                                    <input id="other" class="radio-box" name="gender" type="radio"
                                        :value="GENDER.OTHER.VALUE" v-model="employee.Gender">
                                    <label class="pd-l-8" for="other">{{ EMPLOYEEDETAIL_TITLE.OTHER }}</label>
                                </div>
                            </div>
                        </div>
                        <div class="text-field popup-item-260 pd-b-14">
                            <label class="pd-b-8 popup-lable" :title="TOOLTIP.IDENTITYNUMBER">{{
                                    EMPLOYEEDETAIL_TITLE.IDENTITYNUMBER
                            }}</label>
                            <input tabindex="7" class="input" type="text" v-model="employee.IdentityNumber">
                        </div>
                        <div class="text-field popup-item-160 pd-l-8 pd-b-14">
                            <label class="pd-b-8 popup-lable">{{ EMPLOYEEDETAIL_TITLE.IDENTITYISSUEDATE }}</label>
                            <input tabindex="8" class="input" type="date" v-model="employee.IdentityIssueDate">
                        </div>
                        <div class="text-field popup-item-428">
                            <label class="pd-b-8 popup-lable">{{ EMPLOYEEDETAIL_TITLE.IDENTITYISSUEPLACE }}</label>
                            <input tabindex="10" class="input" type="text" v-model="employee.IdentityIssuePlace">
                        </div>
                    </div>
                </div>
                <div class="half-block-horizontal">
                    <div class="text-field popup-item-row pd-b-14">
                        <label class="pd-b-8 popup-lable">{{ EMPLOYEEDETAIL_TITLE.ADDRESS }}</label>
                        <input tabindex="11" class="input" type="text" v-model="employee.Address">
                    </div>
                    <div class="text-field popup-item-240 pd-b-14">
                        <label class="pd-b-8 popup-lable" :title="TOOLTIP.PHONENUMBER">{{
                                EMPLOYEEDETAIL_TITLE.PHONENUMBER
                        }}</label>
                        <input tabindex="12" class="input" type="text" v-model="employee.PhoneNumber">
                    </div>
                    <div class="text-field popup-item-240 pd-l-8 pd-b-14">
                        <label class="pd-b-8 popup-lable" :title="TOOLTIP.LANDLINENUMBER">{{
                                EMPLOYEEDETAIL_TITLE.LANDLINENUMBER
                        }}</label>
                        <input tabindex="13" class="input" type="text" v-model="employee.LandlineNumber">
                    </div>
                    <div class="text-field popup-item-240 pd-l-8 pd-b-14">
                        <label class="pd-b-8 popup-lable">{{ EMPLOYEEDETAIL_TITLE.EMAIL }}</label>
                        <input tabindex="14" type="text" class="input" v-model="employee.Email">
                    </div>
                    <div class="text-field popup-item-240">
                        <label class="pd-b-8 popup-lable">{{ EMPLOYEEDETAIL_TITLE.BANKACCOUNTNUMBER }}</label>
                        <input tabindex="15" class="input" type="text" v-model="employee.BankAccountNumber">
                    </div>
                    <div class="text-field popup-item-240 pd-l-8">
                        <label class="pd-b-8 popup-lable">{{ EMPLOYEEDETAIL_TITLE.BANKNAME }}</label>
                        <input tabindex="16" class="input" type="text" v-model="employee.BankName">
                    </div>
                    <div class="text-field popup-item-240 pd-l-8">
                        <label class="pd-b-8 popup-lable">{{ EMPLOYEEDETAIL_TITLE.BANKBRANCHNAME }}</label>
                        <input tabindex="17" class="input" type="text" v-model="employee.BankBranchName">
                    </div>
                </div>
            </div>
            <div class="popup__footer">
                <button tabindex="18" @click="hideEmployeeDetail" class="light-btn btn-cancel">{{
                        EMPLOYEEDETAIL_TITLE.CANCEL
                }}</button>
                <div class="space"></div>
                <button tabindex="19" @click="saveData" class="light-btn">{{ EMPLOYEEDETAIL_TITLE.SAVE }}</button>
                <button tabindex="20" @keydown.tab="tabFocus" @click="saveAndAdd" class="btn">{{
                        EMPLOYEEDETAIL_TITLE.SAVE_AND_ADD
                }}</button>
            </div>
        </div>
    </div>
    <MISAWarning :myWarnings="myWarnings" :hideWarning="hideWarning" v-if="isShowWarning"></MISAWarning>
</template>

<script>
import { GENDER } from "../../scripts/enums.js";
import { EMPLOYEEDETAIL_TITLE } from '../../scripts/resources.js';
import { TOOLTIP } from '../../scripts/resources.js';
import { TOAST_CONTENT } from "../../scripts/resources.js";
import MISAWarning from '../../components/base/MISAWarning.vue';
import { ERROR_NAME } from "../../scripts/resources.js";
import { RESULT_CODE } from "../../scripts/enums.js";
import axios from 'axios';
export default {
    name: "EmployeeDetail",
    components: { MISAWarning },
    props: ["hideEmployeeDetail", "employeeSelected", "showToast"],
    created() {
        this.employee = this.employeeSelected;
        // L???y d??? li???u c??c ph??ng ban
        this.loadDepartment();
        // N???u l?? th??m m???i nh??n vi??n th?? l???y m?? nh??n vi??n m???i nh???t
        if (!this.employeeSelected.EmployeeID) {
            this.title = EMPLOYEEDETAIL_TITLE.ADD_EMPLOYEE;
            this.getNewEmployeeCode();
        }
        // Format l???i d??? li???u ng??y th??ng
        this.employee.DateOfBirth = this.toDate(this.employee.DateOfBirth);
        this.employee.IdentityIssueDate = this.toDate(this.employee.IdentityIssueDate);
    },
    mounted() {
        // Focus v??o ?? nh???p li???u ?????u ti??n
        this.$refs.inputEmployeeCode.focus();
    },
    methods: {
        /**
         * L???y d??? li???u c??c ph??ng ban
         * Author: HVTu (20/11/2022)
         */
        loadDepartment() {
            axios.get("http://localhost:58234/api/v1/Departments")
                .then(res => {
                    this.departments = res.data;
                })
                .catch(error => {
                    console.log(error);
                    this.resultHandle.value = RESULT_CODE.EXCEPTION;
                    this.resultHandle.content = ERROR_NAME.EXCEPTION;
                    this.showToast(this.resultHandle);
                })
        },
        /**
         * L???y m?? nh??n vi??n m???i nh???t
         * Author: HVTu (20/11/2022)
         */
        getNewEmployeeCode() {
            axios.get("http://localhost:58234/api/v1/Employees/NewEmployeeCode")
                .then(res => {
                    this.employee.EmployeeCode = res.data;
                })
                .catch(error => {
                    console.log(error);
                    this.resultHandle.value = RESULT_CODE.EXCEPTION;
                    this.resultHandle.content = ERROR_NAME.EXCEPTION;
                    this.showToast(this.resultHandle);
                })
        },
        /**
         * Format d??? li???u ng??y th??ng
         * Author: HVTu (20/11/2022)
         */
        toDate(date) {
            try {
                date = new Date(date);
                // L???y ng??y
                var day = date.getDate();
                day = day < 10 ? `0${day}` : day;
                // L???y th??ng
                var month = date.getMonth() + 1;
                month = month < 10 ? `0${month}` : month;
                // L???y n??m
                var year = date.getFullYear();
                return `${year}-${month}-${day}`;
            } catch (error) {
                console.log(error);
                return "";
            }
        },
        /**
         * T???o focus l???i ?? nh???p li???u ?????u ti??n khi nh???n ph??m tab ??? n??t cu???i c??ng
         * Author: HVTu (21/11/2022)
         */
        tabFocus() {
            try {
                this.$refs.firstFocus.focus();
            } catch (error) {
                console.log(error);
            }
        },
        /**
        * Hi???n th??? c???nh b??o
        * Author: HVTu (17/11/2022)
        */
        showWarning() {
            try {
                this.isShowWarning = true;
            } catch (error) {
                console.log(error);
            }
        },
        /**
         * ???n c???nh b??o
         * Author: HVTu (17/11/2022)
         */
        hideWarning() {
            try {
                this.isShowWarning = false;
                if (this.isEmployeeCodeNull) {
                    this.$refs.inputEmployeeCode.focus();
                    return;
                }
                if (this.isEmployeeNameNull) {
                    this.$refs.inputEmployeeName.focus();
                }
            } catch (error) {
                console.log(error);
            }
        },
        /**
         * Validate m?? nh??n vi??n
         * Author: HVTu (16/11/2022)
         */
        validateEmployeeCode() {
            try {
                if (!this.employee.EmployeeCode) {
                    this.isEmployeeCodeNull = true;
                    this.titleEmployeeCode = TOOLTIP.EMPLOYEECODE_NOT_NULL;
                    this.myWarnings.push(this.titleEmployeeCode);
                } else {
                    this.isEmployeeCodeNull = false;
                    this.titleEmployeeCode = "";
                }
            } catch (error) {
                console.log(error);
            }
        },
        /**
         * Validate t??n nh??n vi??n
         * Author: HVTu (16/11/2022)
         */
        validateEmployeeName() {
            try {
                if (!this.employee.EmployeeName) {
                    this.isEmployeeNameNull = true;
                    this.titleEmployeeName = TOOLTIP.EMPLOYEENAME_NOT_NULL;
                    this.myWarnings.push(this.titleEmployeeName);
                } else {
                    this.isEmployeeNameNull = false;
                    this.titleEmployeeName = "";
                }
            } catch (error) {
                console.log(error);
            }
        },
        /**
         * Validate ph??ng ban
         * Author: HVTu (16/11/2022)
         */
        validateDepartment() {
            try {
                if (!this.employee.DepartmentID) {
                    this.isDepartmentNull = true;
                    this.titleDepartment = TOOLTIP.DEPARTMENT_NOT_NULL;
                    this.myWarnings.push(this.titleDepartment);
                } else {
                    this.isDepartmentNull = false;
                    this.titleDepartment = "";
                }
            } catch (error) {
                console.log(error);
            }
        },
        /**
         * Validate d??? li???u
         * Author: HVTu (12/11/2022)
         */
        validate() {
            try {
                this.myWarnings = [];
                this.validateEmployeeCode();
                this.validateEmployeeName();
                this.validateDepartment();
            } catch (error) {
                console.log(error);
            }
        },
        /**
         * L??u d??? li???u
         * Author: HVTu (15/11/2022)
         */
        saveData() {
            try {
                this.validate();
                if (this.myWarnings.length > 0) {
                    this.showWarning(this.myWarnings);
                } else {
                    this.employee.Gender = parseInt(this.employee.Gender);
                    this.employee.ModifiedDate = new Date();
                    if (!this.employee.EmployeeID) {
                        this.employee.CreatedDate = new Date();
                        this.addEmployee();
                    } else {
                        this.editEmployee();
                    }
                }
            } catch (error) {
                console.log(error);
            }
        },
        /**
         * L??u th??ng tin nh??n vi??n v?? th??m m???i 1 nh??n vi??n
         */
        saveAndAdd() {
            try {
                this.validate();
                if (this.myWarnings.length > 0) {
                    this.showWarning(this.myWarnings);
                } else {
                    this.employee.Gender = parseInt(this.employee.Gender);
                    this.employee.ModifiedDate = new Date();
                    if (!this.employee.EmployeeID) {
                        this.employee.CreatedDate = new Date();
                        this.addEmployeeAndReset();
                    } else {
                        this.editEmployeeAndReset();
                    }
                }
            } catch (error) {
                console.log(error);
            }
        },
        /**
         * G???i API th??m nh??n vi??n
         * Author: HVTu (23/11/2022)
         */
        addEmployee() {
            axios.post("http://localhost:58234/api/v1/Employees", this.employee)
                .then(res => {
                    console.log("add:" + res.data);
                    this.resultHandle.value = RESULT_CODE.SUCCESS;
                    this.resultHandle.content = TOAST_CONTENT.ADD_EMPLOYEE_SUCCESS;
                    this.showToast(this.resultHandle);
                    this.hideEmployeeDetail();
                })
                .catch(error => {
                    console.log(error);
                    this.handleError(error.response.data);
                })
        },
        /**
         * G???i API th??m nh??n vi??n r???i reset l???i form th??m nh??n vi??n
         * Author: HVTu (23/11/2022)
         */
        addEmployeeAndReset() {
            axios.post("http://localhost:58234/api/v1/Employees", this.employee)
                .then(res => {
                    console.log("add:" + res.data);
                    this.resultHandle.value = RESULT_CODE.SUCCESS;
                    this.resultHandle.content = TOAST_CONTENT.ADD_EMPLOYEE_SUCCESS;
                    this.showToast(this.resultHandle);
                    this.employee = {};
                    this.getNewEmployeeCode();
                })
                .catch(error => {
                    console.log(error);
                    this.handleError(error.response.data);
                })
        },
        /**
         * G???i API s???a th??ng tin nh??n vi??n
         * Author: HVTu (23/11/2022)
         */
        editEmployee() {
            axios.put(`http://localhost:58234/api/v1/Employees/${this.employee.EmployeeID}`, this.employee)
                .then(res => {
                    console.log("update: " + res.data);
                    this.resultHandle.value = RESULT_CODE.SUCCESS;
                    this.resultHandle.content = TOAST_CONTENT.EDIT_EMPLOYEE_SUCCESS;
                    this.showToast(this.resultHandle);
                    this.hideEmployeeDetail();
                })
                .catch(error => {
                    console.log(error);
                    this.handleError(error.response.data);
                })
        },
        /**
         * G???i API s???a th??ng tin nh??n vi??n r???i reset l???i form nh??n vi??n
         * Author: HVTu (23/11/2022)
         */
        editEmployeeAndReset() {
            axios.put(`http://localhost:58234/api/v1/Employees/${this.employee.EmployeeID}`, this.employee)
                .then(res => {
                    console.log("update: " + res.data);
                    this.resultHandle.value = RESULT_CODE.SUCCESS;
                    this.resultHandle.content = TOAST_CONTENT.EDIT_EMPLOYEE_SUCCESS;
                    this.showToast(this.resultHandle);
                    this.employee = {};
                    this.getNewEmployeeCode();
                })
                .catch(error => {
                    console.log(error);
                    this.handleError(error.response.data);
                })
        },
        /**
         * X??? l?? l???i server tr??? v???
         * Author: HVTu (13/12/2022)
         */
        handleError(error) {
            switch (error.errorCode) {
                case RESULT_CODE.EXCEPTION:
                    this.myWarnings.push(ERROR_NAME.EXCEPTION);
                    this.isShowWarning = true;
                    break;
                case RESULT_CODE.DUPLICATECODE:
                    this.myWarnings.push(ERROR_NAME.DUPLICATECODE_PREFIXES + this.employee.EmployeeCode + ERROR_NAME.DUPLICATECODE_SUFFIXES);
                    this.isShowWarning = true;
                    break;
                case RESULT_CODE.INVALIDDATA:
                    this.myWarnings.push(ERROR_NAME.INVALIDDATA);
                    this.isShowWarning = true;
                    break;
            }
        }
    },
    data() {
        return {
            GENDER,
            ERROR_NAME,
            EMPLOYEEDETAIL_TITLE,
            TOOLTIP,
            TOAST_CONTENT,
            RESULT_CODE,
            employee: {
                EmployeeID: "",
                EmployeeCode: "",
                EmployeeName: "",
                Gender: null,
                DateOfBirth: "",
                DepartmentID: "",
                JobPosition: "",
                IdentityNumber: "",
                IdentityIssueDate: "",
                IdentityIssuePlace: "",
                Address: "",
                PhoneNumber: "",
                LandlineNumber: "",
                Email: "",
                BankAccountNumber: "",
                BankName: "",
                BankBranchName: "",
                CreatedBy: "",
                CreatedDate: "",
                ModifiedBy: "",
                ModifiedDate: "",
            },
            title: EMPLOYEEDETAIL_TITLE.EDIT_EMPLOYEE,
            isEmployeeCodeNull: false,
            titleEmployeeCode: '',
            isEmployeeNameNull: false,
            titleEmployeeName: '',
            isDepartmentNull: false,
            titleDepartment: '',
            isShowWarning: false,
            departments: [],
            myWarnings: [],
            resultHandle: {
                value: null,
                content: ""
            }
        }
    },
}
</script>

<style scoped>
.border-red {
    border-color: #ff0000;
}
</style>