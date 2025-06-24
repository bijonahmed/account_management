<template>
    <div class="main-wrapper">
        <div class="page-wrapper">
            <Nav />
            <div class="page-content">
                <div class="alert-primary" role="alert">
                    <div class="row">
                        <div class="col-md-10">Edit</div>
                        <div class="col-md-2">
                            <router-link to="/invoice/invoice-list-others"><span
                                    stye="text-align:center;">Back</span></router-link>
                        </div>
                    </div>
                </div><br>
                <div class="row">
                    <div class="col-12">
                        <select name="customer_id" v-model="insertData.customer_id" class="form-select">
                            <option value="">Select</option>
                            <option v-for='data in customerlist' :value='data.customer_id'>{{ data.name }}</option>
                        </select>
                    </div>
                </div>
                <br>
                <div class="alert-primary" role="alert">
                    <div class="col-md-6">
                        Customer Information:<br>
                        Name : {{ customerInfo.customername }}, Phone :{{ customerInfo.customerphone }}<br>Address : {{
                            customerInfo.customeraddres }}
                    </div>
                </div>
                <br>
                <div class="row">
                    <div class="col-md-12 grid-margin stretch-card">
                        <div class="card">
                            <div class="card-body">
                                <div v-for="(errorArray, idx) in notifmsg" :key="idx">
                                    <div v-for="(allErrors, idx) in errorArray" :key="idx">
                                        <span class="text-danger">{{ allErrors }} </span>
                                    </div>
                                </div>

                                <form @submit.prevent="addItem()" id="formrest" class="forms-sample"
                                    enctype="multipart/form-data">

                                    <input type="hidden" class="form-control others_inv_id"
                                        v-model="insertData.others_inv_id" id="others_inv_id" autocomplete="off">
                                    <input type="hidden" class="form-control customer_id" name="customer_id"
                                        id="customer_id">

                                    <div class="row">
                                        <div class="col-md-6">
                                            <div class="row mb-1">
                                                <label for="exampleInputUsername2"
                                                    class="col-sm-4 col-form-label">Invoice NO</label>
                                                <div class="col-sm-8">
                                                    <input type="text" class="form-control invoice_no"
                                                        v-model="insertData.invoice_no" id="invoice_no"
                                                        autocomplete="off">
                                                </div>
                                            </div>


                                            <div class="row mb-1">

                                                <label for="exampleInputUsername2"
                                                    class="col-sm-4 col-form-label">Invoice Date</label>
                                                <div class="col-sm-8">
                                                    <input type="date" class="form-control invoice_date"
                                                        v-model="insertData.invoice_date" id="invoice_date"
                                                        placeholder="Invoice Date" autocomplete="off">
                                                </div>
                                            </div>
                                            <div class="row mb-1">
                                                <label for="exampleInputUsername2"
                                                    class="col-sm-4 col-form-label">Supplier</label>
                                                <div class="col-sm-8">
                                                    <select name="sulipper_id" v-model="insertData.sulipper_id"
                                                        class="form-select sulipper_id">
                                                        <option value="">Select</option>
                                                        <option v-for='data in supplierlist' :value='data.sulipper_id'>
                                                            {{ data.name }}</option>
                                                    </select>
                                                </div>
                                            </div>


                                            <div class="row mb-1">
                                                <label for="exampleInputUsername2"
                                                    class="col-sm-4 col-form-label">Beneficiary name</label>
                                                <div class="col-sm-8">
                                                    <input type="text" class="form-control beneficiary_name"
                                                        v-model="insertData.beneficiary_name" id="beneficiary_name"
                                                        autocomplete="off">
                                                </div>
                                            </div>



                                            <div class="row mb-1">
                                                <label for="exampleInputUsername2"
                                                    class="col-sm-4 col-form-label">Reason</label>
                                                <div class="col-sm-8">
                                                    <input type="text" class="form-control reason"
                                                        v-model="insertData.reason" id="reason" autocomplete="off">
                                                </div>
                                            </div>

                                            <input type="hidden" class="form-control customer_id" name="customer_id"
                                                id="customer_id">

                                        </div>
                                        <div class="col-md-6">

                                            <div class="row mb-1">
                                                <label for="exampleInputUsername2"
                                                    class="col-sm-5 col-form-label">Amount</label>
                                                <div class="col-sm-7">
                                                    <input type="text" autocomplete="off"
                                                        class="form-control amount onlyNumbersInput"
                                                        v-model="insertData.amount" id="amount" placeholder="0.00">
                                                </div>
                                            </div>

                                            <div class="row mb-1">
                                                <label for="exampleInputUsername2"
                                                    class="col-sm-5 col-form-label">Charge/Fee</label>
                                                <div class="col-sm-7">
                                                    <input type="text" class="form-control charge_fee onlyNumbersInput"
                                                        v-model="insertData.charge_fee" id="charge_fee"
                                                        autocomplete="off" placeholder="0.00">
                                                </div>
                                            </div>

                                            <div class="row mb-1">
                                                <label for="exampleInputUsername2" class="col-sm-5 col-form-label">Total
                                                    Amount</label>
                                                <div class="col-sm-7">
                                                    <input type="text"
                                                        class="form-control total_amount onlyNumbersInput"
                                                        v-model="insertData.total_amount" id="total_amount"
                                                        autocomplete="off" placeholder="0.00">
                                                </div>
                                            </div>

                                            <div class="row mb-1">
                                                <label for="exampleInputUsername2"
                                                    class="col-sm-5 col-form-label">Amount Paid</label>
                                                <div class="col-sm-7">
                                                    <input type="text" class="form-control amount_paid onlyNumbersInput"
                                                        v-model="insertData.amount_paid" id="amount_paid"
                                                        placeholder="0.00" autocomplete="off">
                                                </div>
                                            </div>

                                            <div class="row mb-1">
                                                <label for="exampleInputUsername2"
                                                    class="col-sm-5 col-form-label">Amount Remaining </label>
                                                <div class="col-sm-7">
                                                    <input type="text"
                                                        class="form-control amount_remaining onlyNumbersInput"
                                                        v-model="insertData.amount_remaining" id="amount_remaining"
                                                        placeholder="0.00" autocomplete="off">
                                                </div>
                                            </div>


                                            <div class="row mb-1">
                                                <label for="exampleInputUsername2"
                                                    class="col-sm-5 col-form-label">Additional Notes</label>
                                                <div class="col-sm-7">
                                                    <input type="text" class="form-control additional_notes"
                                                        v-model="insertData.additional_notes" id="additional_notes"
                                                        placeholder="Additional Notes" autocomplete="off">
                                                </div>
                                            </div>

                                            <button type="submit"
                                                class="btn btn-primary me-2 w-100 submit_btn">Submit</button>
                                        </div>
                                    </div>

                                </form>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <Footer />
        </div>
    </div>
</template>

<script>
// import Nav from '../../components/Nav.vue';
// import Sidebar from '../../components/Sidebar.vue';
export default {
    components: {
        // Nav,
        // Sidebar
    },
    data() {
        return {
            cdata: {
                company_name: '',
                address: '',
                email: '',
                tel: '',
            },
            customerInfo: {
                customer_id: '',
                customername: '',
                customeraddres: '',
                customerphone: '',
            },
            /*
            insertData: {
                invoice_date: '',
                customer_id: '',
                others_inv_id: '',
                purpose: '',
                net_amount: '',
                customer_amount: '',
                amount_paid: '',
                profit: '',
                due_amount: '',
            },
            */
            insertData: {
                invoice_date: '',
                customer_id: '',
                invoice_no: '',
                sulipper_id: '',
                others_inv_id: '',
                beneficiary_name: '',
                reason: '',
                purpose: '',
                charge_fee: '',
                amount: '',
                total_amount: '',
                amount_paid: '',
                amount_remaining: '',
                additional_notes: ''
            },

            notifmsg: '',
            customerlist: [],
            supplierlist: [],
        }
    },
    created() {
        this.findRowId();
        this.getCustomerlist();
        this.selectCompany();
        this.allSuppliers();
        this.allSuppliers();
    },
    methods: {
        addItem() {
            const formData = new FormData();
            var cus_id = $('.customer_id').val();
            formData.append('sulipper_id', this.insertData.sulipper_id);
            formData.append('beneficiary_name', this.insertData.beneficiary_name);
            formData.append('invoice_no', this.insertData.invoice_no);
            formData.append('invoice_date', this.insertData.invoice_date);
            formData.append('others_inv_id', this.insertData.others_inv_id);
            formData.append('reason', this.insertData.reason);
            formData.append('customer_id', cus_id);
            formData.append('amount', this.insertData.amount);
            formData.append('charge_fee', this.insertData.charge_fee);
            formData.append('total_amount', this.insertData.total_amount);
            formData.append('amount_paid', this.insertData.amount_paid);
            formData.append('amount_remaining', this.insertData.amount_remaining);
            formData.append('additional_notes', this.insertData.additional_notes);
            const headers = {
                'Content-Type': 'multipart/form-data'
            };
            axios.post('/api/category/saveInvoiceOthers',
                formData, {
                headers
            }).then((res) => {
                $('#formrest')[0].reset();
                Swal.fire({
                    position: 'top-end',
                    icon: 'success',
                    title: 'Data has been saved!',
                    showConfirmButton: false,
                    timer: 1500
                })
                this.$router.push('/invoice/invoice-list-others');
            }).catch(e => {
                this.notifmsg = e.response.data
            });
        },
        allSuppliers() {
            axios.get('/api/category/allsuppliders').then(response => {
                this.supplierlist = response.data.data;
            }).catch(error => {
                //  console.log(error);
            });
        },
        selectCompany() {
            this.axios
                .get(`/api/user/getCompanySetting`)
                .then((response) => {
                    this.cdata.company_name = response.data.data.company_name;
                    this.cdata.address = response.data.data.address;
                    this.cdata.email = response.data.data.email;
                    this.cdata.tel = response.data.data.tel;
                });
        },
        findRowId() {
            this.axios
                .get(`/api/category/editInvoiceOthersInvId/${this.$route.params.id}`)
                .then((response) => {
                    this.insertData.invoice_no = response.data.data.invoice_no;
                    this.insertData.sulipper_id = response.data.data.sulipper_id;
                    this.insertData.customer_id = response.data.data.customer_id;
                    this.insertData.others_inv_id = response.data.data.others_inv_id;
                    this.insertData.beneficiary_name = response.data.data.beneficiary_name;
                    this.insertData.reason = response.data.data.reason;
                    this.insertData.additional_notes = response.data.data.additional_notes;

                    this.insertData.invoice_date = response.data.data.invoice_date;
                    $(".customer_id").val(response.data.data.customer_id);


                    this.insertData.amount = response.data.data.amount;
                    this.insertData.total_amount = response.data.data.total_amount;
                    this.insertData.amount_paid = response.data.data.amount_paid;
                    this.insertData.charge_fee = response.data.data.charge_fee;
                    this.insertData.amount_remaining = response.data.data.amount_remaining;
                    
                    //header
                    this.customerInfo.customername = response.data.data.customername;
                    this.customerInfo.customeraddres = response.data.data.addres;
                    this.customerInfo.customerphone = response.data.data.phone;

                });
        },
        payType() {
            let payment_type = this.insertData.payment_type
            this.insertData.p_type = payment_type;
        },
        getCustomerlist() {
            axios.get(`/api/category/allCustomerOthers`).then(response => {
                this.customerlist = response.data.data;
            }).catch(error => {
                //  console.log(error);
            });
        },

  allSuppliers() {
            axios.get('/api/category/othersSupplier').then(response => {
                this.supplierlist = response.data.data;
            }).catch(error => {
                //  console.log(error);
            });
        },

        calculated() {
            let result = parseFloat(this.insertData.receiving_amount) / parseFloat(this.insertData.rate) + parseFloat(this.insertData.fees) + parseFloat(this.insertData.others_fees);
            console.log(result.toFixed(2));
            this.insertData.total_amount = result.toFixed(2);
            this.profitCalculate();
        },
       

    }
}
</script>
