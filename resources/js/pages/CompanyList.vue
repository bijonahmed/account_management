<template>
    <div class="main-wrapper">
        <div class="page-wrapper">
            <Nav />
            <div class="page-content">
                <div class="alert-primary" role="alert">
                    <div class="row">
                        <div class="col-md-10">Company List</div>

                    </div>
                </div><br>
                <!-- <span class="loader"></span> -->

                <div class="row">
                    <div class="col-md-10">

                    </div>

                    <div class="col-md-2">
                        <div class="input-group mb-3">
                            <button class="btn btn-primary w-100" @click="addNew">Add New</button>
                            <!-- <button class="btn btn-primary w-100" @click="closeModal">hid modal New</button> -->



                        </div>
                    </div>
                </div>
                <!-- START DELETE MODAL -->
                <!-- Button trigger modal -->
                <button type="button" class="btn btn-primary add-modal" data-bs-toggle="modal"
                    data-bs-target="#staticBackdrop" style="display:none">
                    Add
                </button>
                <!-- delete Modal -->
                <div class="modal fade" id="staticBackdrop" tabindex="-1" aria-labelledby="staticBackdropLabel"
                    aria-hidden="true">
                    <div class="modal-dialog">
                        <div class="modal-content">
                            <div class="modal-header">
                                <h5 class="modal-title" id="staticBackdropLabel">Add Company</h5>
                                <button type="button" class="btn-close" data-bs-dismiss="modal"
                                    aria-label="Close"></button>
                            </div>
                            <div class="modal-body">


                                <div class="card">
                                    <div class="card-body">
                                        <div v-for="(errorArray, idx) in notifmsg" :key="idx">
                                            <div v-for="(allErrors, idx) in errorArray" :key="idx">
                                                <span class="text-danger">{{ allErrors }} </span>
                                            </div>
                                        </div>
                                        <form @submit.prevent="saveData()" id="formrest" class="forms-sample"
                                            enctype="multipart/form-data">
                                            <div class="row">
                                                <div class="d-flex justify-content-center">
                                                    <div class="spinner-border loading" role="status"
                                                        style="display:none">
                                                        <span class="visually-hidden">Loading...</span>
                                                    </div>
                                                </div>
                                                <div class="col-sm-12">

                                                    <div class="row mb-3">
                                                        <label for="exampleInputUsername2"
                                                            class="col-sm-4 col-form-label">Company Name</label>
                                                        <div class="col-sm-8">
                                                            <input type="text" class="form-control"
                                                                v-model="cdata.company_name" id="company_name"
                                                                name="company_name" placeholder="Company Name">
                                                                <input type="hidden" class="form-control" v-model="cdata.id">
                                                        </div>
                                                    </div>

                                                    <div class="row mb-3">
                                                        <label for="exampleInputUsername2"
                                                            class="col-sm-4 col-form-label">Status</label>
                                                        <div class="col-sm-8">
                                                            <select name="status" v-model="cdata.status"
                                                                class="form-select">
                                                                <option value="1" selected>Active</option>
                                                                <option value="0">Inactive</option>
                                                            </select>
                                                        </div>
                                                    </div>

                                                </div>

                                            </div>
                                            <button type="submit"
                                                class="btn btn-primary me-2 w-100 submit_btn">Submit</button>
                                        </form>
                                    </div>
                                </div>

                            </div>
                            <div class="modal-footer d-none">
                                <button type="button" class="btn btn-secondary clseModal"
                                    data-bs-dismiss="modal">Close</button>
                                <!-- <button type="submit" class="btn btn-danger" @click="saveData">Submit</button> -->
                            </div>
                        </div>
                    </div>
                </div>
                <!-- END DELETE MODAL -->
                <div class="col-md-12">
                    <div class="table-responsive w-100">

                        <table class="table table-striped table-hover table-sm">
                            <thead>
                                <tr>
                                    <th>Company Name</th>
                                    <th>Status</th>
                                    <th>Actions</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(company, index) in companyList" :key="index">
                                    <td>{{ company.company_name }}</td>
                                    <td>{{ company.status === 1 ? 'Active' : 'Inactive' }}</td>
                                    <td>
                                        <!-- Add any actions here, e.g., Edit, Delete -->
                                        <button @click="editCompany(company)"
                                            class="btn btn-primary me-2 w-100">Edit</button>

                                    </td>
                                </tr>
                            </tbody>
                        </table>

                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import $ from "jquery";

export default {
    data() {
        return {
            cdata: {
                id: '',
                company_name: '',
                status: 1,
            },
            notifmsg: '',
            companyList: [],
        }
    },
    created() {
        this.getCompanyListList();

    },
    methods: {
        addNew() {
            $(".add-modal").click();
        },

        closeModal() {
            $(".clseModal").click();
        },

        getCompanyListList() {
            axios.get('/api/company/getcountryList').then(response => {
                this.companyList = response.data.data;
            });
        },


        editCompany(company) {
            const company_id = company.id;
            axios.get('/api/company/getcompanyrow', {
                params: {
                    company_id: company_id
                }
            })
                .then(response => {
                    this.addNew();
                    console.log(":===" + response.data.data.company_name);
                    this.cdata.company_name = response.data.data.company_name;
                    this.cdata.id = response.data.data.id;
                    this.cdata.status = response.data.data.status;
                    //this.companyList = response.data.data;
                })
                .catch(error => {
                    console.error('Error fetching company row:', error);
                });
        },


        saveData() {
            const formData = new FormData();
            formData.append('id', this.cdata.id);
            formData.append('company_name', this.cdata.company_name);
            formData.append('status', this.cdata.status);
            const headers = {
                'Content-Type': 'multipart/form-data'
            };
            axios.post('/api/company/companyInsert',
                formData, {
                headers
            }).then((res) => {
                this.closeModal();
                this.getCompanyListList();
                $('#formrest')[0].reset();
                this.cdata.company_name = "";
                Swal.fire({
                    position: 'top-end',
                    icon: 'success',
                    title: 'Data has been saved!',
                    showConfirmButton: false,
                    timer: 1500
                })

                // return false;
            });
        },
    },
}
</script>
 