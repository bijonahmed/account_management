<template>
    <div class="main-wrapper">
        <div class="page-wrapper">
            <Nav />
            <div class="page-content">
                <div class="alert-primary" role="alert" style="margin-bottom:10px;">
                    <div class="row">
                        <div class="col-md-8">Due Report</div>
                    </div>
                </div>


                <form @submit.prevent="filterReport()" id="formrest" class="forms-sample" enctype="multipart/form-data">
                    <div class="row">
                        <div class="col-md-6">
                            <div class="input-group mb-3">
                                <select name="status" v-model="selected_type" class="form-select"
                                    @change="serviceWiseCustomerSupplier">
                                    <option value="">Select Type</option>
                                    <option value="1">Travel</option>
                                    <option value="2">Money Transfer</option>
                                    <option value="3">Consular</option>
                                    <option value="4">Others</option>
                                </select>
                            </div>
                        </div>

                        <div class="col-md-6">
                            <div class="input-group mb-3">
                                <select class='form-control form-select customer_id' v-model="customer_id">
                                    <option value=''>All Customer</option>
                                    <option v-for='data in customers' :value='data.customer_id'>{{ data.name }}</option>
                                </select>
                            </div>
                        </div>


                        <div class="col-md-3">
                            <div class="input-group mb-3">
                                <select class='form-control form-select sulipper_id' v-model="sulipper_id">
                                    <option value=''>All Supplier</option>
                                    <option v-for='data in supliers' :value='data.sulipper_id'>{{ data.name }}</option>
                                </select>
                            </div>
                        </div>

                        <div class="col-md-2">
                            <div class="input-group mb-3">
                                <input type="date" class="form-control frm_date" id="frm_date" v-model="frm_date"
                                    placeholder="From Date">
                            </div>
                        </div>
                        <div class="col-md-2">
                            <div class="input-group mb-3">
                                <!-- <Datepicker class="form-control to_date" placeholder="To Date"></Datepicker>   -->
                                <input type="date" class="form-control to_date" id="datepicker" placeholder="To Date"
                                    v-model="to_date">
                            </div>
                        </div>

                        <div class="col-md-4">
                            <div class="input-group mb-3">
                                <!-- <Datepicker class="form-control to_date" placeholder="To Date"></Datepicker>   -->
                                <button type="submit" class="btn btn-primary w-100">Submit</button>
                            </div>
                        </div>
                    </div>
                </form>


                <div class="d-flex justify-content-center">
                    <div class="spinner-border loadingvideo" role="status" style="display:none">
                        <span class="visually-hidden">Loading...</span>
                    </div>
                </div>


                <div v-if="selected_type == 1">
                    <!-- For Travel Report -->
                    <div class="table-responsive">
                        <table id="datatable" class="table table-striped table-hover table-sm report-table">
                            <thead>
                                <tr>
                                    <th>
                                        <input type="checkbox" v-model="selectAll" @change="toggleAllTravel">
                                    </th>
                                    <th>Invoice ID</th>
                                    <th>Datetime</th>
                                    <th>Customer Name</th>
                                    <th>Customer Phone</th>
                                    <th>Suplier Name</th>
                                    <th>Create By</th>
                                    <th class="text-end">Due Amount</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(data, index) in report_travel" :key="index">
                                    <td>
                                        <input type="checkbox" v-model="selectedInvoicesTravel"
                                            :value="data.invoice_id">
                                    </td>
                                    <td>{{ data.inv_id }}</td>
                                    <td>{{ data.invoice_date }}</td>
                                    <td>{{ data.customer_name }}</td>
                                    <td>{{ data.phone }}</td>
                                    <td>{{ data.suplier_name }}</td>
                                    <td>{{ data.name }}</td>
                                    <td class="text-end">{{ data.due_amount }}</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                    <div class="row text-end">
                        <span style="font-weight: bold;color:green;">Total Due: {{ total_due_travel }}</span>
                    </div>
                    <div class="continaer">
                        <center><button type="button" class="btn btn-primary w-100 paytment_btn" @click="paymentModal"
                                style="display: none;">Payment</button></center>
                    </div>
                </div>


                <div v-if="selected_type == 2">
                    <!-- For Money Transfer Report -->
                    <div class="table-responsive">
                        <table id="datatable" class="table table-striped table-hover table-sm report-table">
                            <thead>
                                <tr>
                                    <th>
                                        <input type="checkbox" v-model="selectAll" @change="toggleAllMoneyTransfer">
                                    </th>
                                    <th>Invoice ID </th>
                                    <th>Datetime</th>
                                    <th>Customer Name</th>
                                    <th>Customer Phone</th>
                                    <th>Suplier</th>
                                    <th>Create By</th>
                                    <th class="text-end">Due Amount</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for='data in money_report'>
                                    <td>
                                        <input type="checkbox" v-model="selectedInvoicesMoneyTransfer"
                                            :value="data.mone_transfer_id">
                                    </td>
                                    <td>{{ data.mone_transfer_id }}</td>
                                    <td>{{ data.invoice_date }}</td>
                                    <td>{{ data.customer_name }}</td>
                                    <td>{{ data.phone }}</td>
                                    <td>{{ data.suplier_name }}</td>
                                    <td>{{ data.name }}</td>
                                    <td class="text-end">
                                        {{ (data.receiving_amount / data.rate - data.customer_deposit).toFixed(2) }}
                                        <!-- {{ data.due_amount }} -->
                                    
                                    
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                    <div class="row">
                        <div class="text-end">
                            <span style="font-weight: bold;color:green;">Total Due:
                                {{
                                    isNaN(parseFloat(money_total_due)) ? '0.00' :
                                        parseFloat(money_total_due).toFixed(2) }}





                            </span>
                        </div>
                        <div class="continaer">
                            <center><button type="submit" class="btn btn-primary w-100 paytment_btn"
                                    style="display: none;" @click="paymentModal">Payment</button></center>
                        </div>

                    </div>

                </div>

                <div v-if="selected_type == 3">
                    <!-- For Consular Report -->
                    <div class="table-responsive">
                        <table id="datatable" class="table table-striped table-hover table-sm report-table">
                            <thead>
                                <tr>
                                    <th>
                                        <input type="checkbox" v-model="selectAll" @change="toggleAllConsular">
                                    </th>
                                    <th>Invoice ID</th>
                                    <th>Date</th>
                                    <th>Customer Name</th>
                                    <th>Purpose</th>
                                    <th>Net Amount</th>
                                    <th>Customer Amount</th>
                                    <th>Amount Paid</th>
                                    <th>Amount Due</th>
                                    <th>Suplier</th>
                                    <th>Create By</th>

                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for='data in consular_report'>

                                    <td>
                                        <input type="checkbox" v-model="selectedInvoicesConsular"
                                            :value="data.consular_inv_id">
                                    </td>
                                    <td>{{ data.consular_inv_id }}</td>
                                    <td>{{ data.invoice_date }}</td>
                                    <td>{{ data.customer_name }}</td>
                                    <td>{{ data.purpose }}</td>
                                    <td>{{ data.net_amount }}</td>
                                    <td>{{ data.customer_amount }}</td>
                                    <td>{{ data.amount_paid }}</td>
                                    <td>{{ data.due_amount }}</td>
                                    <td>{{ data.suplier_name }}</td>
                                    <td>{{ data.name }}</td>


                                </tr>
                            </tbody>
                        </table>
                    </div>
                    <div class="row text-end">
                        <span style="font-weight: bold;color:green;">Total Due: {{ consular_total_due }}</span>

                        <div class="continaer">
                            <center><button type="submit" class="btn btn-primary w-100 paytment_btn"
                                    style="display: none;" @click="paymentModal">Payment</button></center>
                        </div>
                    </div>


                </div>

                <div v-if="selected_type == 4">
                    <!-- For Others Report -->
                    <div class="table-responsive">
                        <table id="datatable" class="table table-striped table-hover table-sm report-table">
                            <thead>
                                <tr>
                                    <th>
                                        <input type="checkbox" v-model="selectAll" @change="toggleAllOthers">
                                    </th>
                                    <th>Date</th>
                                    <th>Customer Name</th>
                                    <th>Reason</th>
                                    <th>Amount</th>
                                    <th>Amount Due</th>
                                    <th>Suplier</th>
                                    <th>Create By</th>

                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for='data in others_report'>

                                    <td>
                                        <input type="checkbox" v-model="selectedInvoicesOthers"
                                            :value="data.others_inv_id">
                                    </td>

                                    <td>{{ data.invoice_date }}</td>
                                    <td>{{ data.customer_name }}</td>
                                    <td>{{ data.reason }}</td>
                                    <td>{{ data.amount }}</td>
                                    <td>{{ data.amount_remaining }}</td>
                                    <td>{{ data.suplier_name }}</td>
                                    <td>{{ data.name }}</td>


                                </tr>
                            </tbody>
                        </table>
                    </div>
                    <div class="row text-end">
                        <span style="font-weight: bold;color:green;">Total Due: {{ other_total_due }}</span>
                        <div class="continaer">
                            <center><button type="submit" class="btn btn-primary w-100 paytment_btn"
                                    style="display: none;" @click="paymentModal">Payment</button></center>
                        </div>
                    </div>
                </div>


                <!-- row -->
            </div>
            <!-- Button trigger modal -->
            <Footer />


            <!-- Others  Modal -->
            <div class="modal fade showMoneyOthersModal" tabindex="-1" aria-hidden="true">
                <div class="modal-dialog">
                    <div class="modal-content">
                        <div class="modal-header">
                            <h5 class="modal-title">Others Supplier Payment</h5>
                            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                        </div>
                        <div class="modal-body">
                            <form>
                                <div class="mb-3">
                                    <label for="paymentDate" class="form-label">Payment Date:</label>
                                    <input type="date" id="paymentDate" class="form-control" v-model="paymentDate">
                                </div>

                                <div class="mb-3">
                                    <label for="supplierSelect" class="form-label">Select Supplier:</label>
                                    <select class='form-control form-select sulipper_id' v-model="sulipper_id">
                                        <option value=''>All Supplier</option>
                                        <option v-for='data in supliers' :value='data.sulipper_id'>{{ data.name }}
                                        </option>
                                    </select>

                                </div>

                                <div class="mb-3">
                                    <label for="amount" class="form-label">Amount:</label>
                                    <input type="text" id="amount" class="form-control" v-model="paymentAmount"
                                        @input="onAmountInput">
                                </div>

                                <div class="mb-3">
                                    <label for="paymentMethod" class="form-label">Payment Method:</label>
                                    <select id="paymentMethod" class="form-select" v-model="paymentMethod">
                                        <option value="">-- Select Payment Method --</option>
                                        <option value="Cash">Cash</option>
                                        <option value="Bank Transfer">Bank Transfer</option>
                                        <option value="Credit Loan">Credit Loan</option>
                                        <option value="Others">Others</option>
                                    </select>
                                </div>
                            </form>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                            <button type="button" class="btn btn-primary"
                                @click="updatePaymentStatusOthers">Save</button>
                        </div>
                    </div>
                </div>
            </div>


            <!-- Consular  Modal -->
            <div class="modal fade showMoneyConsularModal" tabindex="-1" aria-hidden="true">
                <div class="modal-dialog">
                    <div class="modal-content">
                        <div class="modal-header">
                            <h5 class="modal-title">Consular Supplier Payment</h5>
                            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                        </div>
                        <div class="modal-body">
                            <form>
                                <div class="mb-3">
                                    <label for="paymentDate" class="form-label">Payment Date:</label>
                                    <input type="date" id="paymentDate" class="form-control" v-model="paymentDate">
                                </div>

                                <div class="mb-3">
                                    <label for="supplierSelect" class="form-label">Select Supplier:</label>
                                    <select class='form-control form-select sulipper_id' v-model="sulipper_id">
                                        <option value=''>All Supplier</option>
                                        <option v-for='data in supliers' :value='data.sulipper_id'>{{ data.name }}
                                        </option>
                                    </select>

                                </div>

                                <div class="mb-3">
                                    <label for="amount" class="form-label">Amount:</label>
                                    <input type="text" id="amount" class="form-control" v-model="paymentAmount"
                                        @input="onAmountInput">
                                </div>

                                <div class="mb-3">
                                    <label for="paymentMethod" class="form-label">Payment Method:</label>
                                    <select id="paymentMethod" class="form-select" v-model="paymentMethod">
                                        <option value="">-- Select Payment Method --</option>
                                        <option value="Cash">Cash</option>
                                        <option value="Bank Transfer">Bank Transfer</option>
                                        <option value="Credit Loan">Credit Loan</option>
                                        <option value="Others">Others</option>
                                    </select>
                                </div>
                            </form>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                            <button type="button" class="btn btn-primary"
                                @click="updatePaymentStatusConsular">Save</button>
                        </div>
                    </div>
                </div>
            </div>


            <!-- Money Transfer Modal -->
            <div class="modal fade showMoneyTransferModal" tabindex="-1" aria-hidden="true">
                <div class="modal-dialog">
                    <div class="modal-content">
                        <div class="modal-header">
                            <h5 class="modal-title">Money Transfer Supplier Payment</h5>
                            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                        </div>
                        <div class="modal-body">
                            <form>
                                <div class="mb-3">
                                    <label for="paymentDate" class="form-label">Payment Date:</label>
                                    <input type="date" id="paymentDate" class="form-control" v-model="paymentDate">
                                </div>

                                <div class="mb-3">
                                    <label for="supplierSelect" class="form-label">Select Supplier:</label>
                                    <select class='form-control form-select sulipper_id' v-model="sulipper_id">
                                        <option value=''>All Supplier</option>
                                        <option v-for='data in supliers' :value='data.sulipper_id'>{{ data.name }}
                                        </option>
                                    </select>

                                </div>

                                <div class="mb-3">
                                    <label for="amount" class="form-label">Amount:</label>
                                    <input type="text" id="amount" class="form-control" v-model="paymentAmount"
                                        @input="onAmountInput">
                                </div>

                                <div class="mb-3">
                                    <label for="paymentMethod" class="form-label">Payment Method:</label>
                                    <select id="paymentMethod" class="form-select" v-model="paymentMethod">
                                        <option value="">-- Select Payment Method --</option>
                                        <option value="Cash">Cash</option>
                                        <option value="Bank Transfer">Bank Transfer</option>
                                        <option value="Credit Loan">Credit Loan</option>
                                        <option value="Others">Others</option>
                                    </select>
                                </div>
                            </form>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                            <button type="button" class="btn btn-primary"
                                @click="updatePaymentStatusMoneyTransfer">Save</button>
                        </div>
                    </div>
                </div>
            </div>


            <!-- Travel Modal -->
            <div class="modal fade showtravelModal" tabindex="-1" aria-hidden="true">
                <div class="modal-dialog">
                    <div class="modal-content">
                        <div class="modal-header">
                            <h5 class="modal-title">Travel Supplier Payment</h5>
                            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                        </div>
                        <div class="modal-body">
                            <form>
                                <div class="mb-3">
                                    <label for="paymentDate" class="form-label">Payment Date:</label>
                                    <input type="date" id="paymentDate" class="form-control" v-model="paymentDate">
                                </div>

                                <div class="mb-3">
                                    <label for="supplierSelect" class="form-label">Select Supplier:</label>
                                    <select class='form-control form-select sulipper_id' v-model="sulipper_id">
                                        <option value=''>All Supplier</option>
                                        <option v-for='data in supliers' :value='data.sulipper_id'>{{ data.name }}
                                        </option>
                                    </select>

                                </div>

                                <div class="mb-3">
                                    <label for="amount" class="form-label">Amount:</label>
                                    <input type="text" id="amount" class="form-control" v-model="paymentAmount"
                                        @input="onAmountInput">
                                </div>

                                <div class="mb-3">
                                    <label for="paymentMethod" class="form-label">Payment Method:</label>
                                    <select id="paymentMethod" class="form-select" v-model="paymentMethod">
                                        <option value="">-- Select Payment Method --</option>
                                        <option value="Cash">Cash</option>
                                        <option value="Bank Transfer">Bank Transfer</option>
                                        <option value="Credit Loan">Credit Loan</option>
                                        <option value="Others">Others</option>
                                    </select>
                                </div>
                            </form>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
                            <button type="button" class="btn btn-primary"
                                @click="updatePaymentStatusTravel">Save</button>
                        </div>
                    </div>
                </div>
            </div>


        </div>
    </div>
</template>

<script>
import $ from "jquery";
import Datepicker from '../../components/Datepicker.vue';
import datepickerFactory from 'jquery-datepicker'
import {
    useRouter
} from "vue-router"
datepickerFactory($);
export default {
    setup() {
        const router = useRouter()
    },
    components: {
        Datepicker
    },
    data: function () {
        return {
            selectAll: false,
            selectedInvoicesTravel: [],
            selectedInvoicesMoneyTransfer: [],
            selectedInvoicesConsular: [],
            selectedInvoicesOthers: [],
            supliers: [],
            customers: [],
            frm_date: '',
            to_date: '',
            phone: '',
            paymentDate: '',
            paymentMethod: '',
            selected_type: '',
            total_due_travel: '',
            money_total_due: '',
            other_total_due: '',
            consular_total_due: '',
            customer_id: '',
            sulipper_id: '',
            report_travel: [],
            money_report: [],
            others_report: [],
            consular_report: [],
            paymentAmount: '',
        };
    },
    mounted() {
        //  this.customerlist();
        //   $('.frm_date,.to_date').datepicker();
    },

    methods: {
        serviceWiseCustomerSupplier() {
            //  console.log("selected_type" + this.selected_type);
            axios.get('/api/category/serviceWiseAllCustomers', {
                params: {
                    selected_type: this.selected_type,
                }
            })
                .then(response => {
                    this.customers = response.data.customerData;
                    this.supliers = response.data.suplierData;
                })
                .catch(error => {
                    console.error(error);
                });
        },
        filterReport() {
            const serviceType = this.selected_type;

            if (serviceType == 1) {
                this.searchDataTravel();
            }

            if (serviceType == 2) {
                this.searchDataForMoney();
            }

            if (serviceType == 3) {
                this.searchDataConsular();
            }

            if (serviceType == 4) {
                this.searchDataOthers();
            }

        },
        toggleAllTravel() {
            if (this.selectAll) {
                this.selectedInvoicesTravel = this.report_travel.map(item => item.invoice_id);
            } else {
                this.selectedInvoicesTravel = [];
            }
        },

        toggleAllMoneyTransfer() {
            if (this.selectAll) {
                this.selectedInvoicesMoneyTransfer = this.money_report.map(item => item.mone_transfer_id);
            } else {
                this.selectedInvoicesMoneyTransfer = [];
            }
        },
        toggleAllConsular() {
            if (this.selectAll) {
                this.selectedInvoicesConsular = this.consular_report.map(item => item.consular_inv_id);
            } else {
                this.selectedInvoicesConsular = [];
            }
        },

        toggleAllOthers() {
            if (this.selectAll) {
                this.selectedInvoicesOthers = this.others_report.map(item => item.others_inv_id);
            } else {
                this.selectedInvoicesOthers = [];
            }
        },
        paymentModal() {
            const serviceType = this.selected_type;

            if (serviceType == 1) {
                if (this.selectedInvoicesTravel.length > 0) {
                    const modal = new bootstrap.Modal(document.querySelector('.showtravelModal'));
                    modal.show();
                } else {
                    alert('Please select at least one item.');
                }
            }

            if (serviceType == 2) {

                if (this.selectedInvoicesMoneyTransfer.length > 0) {
                    const modal = new bootstrap.Modal(document.querySelector('.showMoneyTransferModal'));
                    modal.show();
                } else {
                    alert('Please select at least one item.');
                }
            }

            if (serviceType == 3) {
                if (this.selectedInvoicesConsular.length > 0) {
                    const modal = new bootstrap.Modal(document.querySelector('.showMoneyConsularModal'));
                    modal.show();
                } else {
                    alert('Please select at least one item.');
                }
            }

            if (serviceType == 4) {

                if (this.selectedInvoicesOthers.length > 0) {
                    const modal = new bootstrap.Modal(document.querySelector('.showMoneyOthersModal'));
                    modal.show();
                } else {
                    alert('Please select at least one item.');
                }


            }

        },

        updatePaymentStatusTravel() {
            const selectedIds = this.selectedInvoicesTravel;
            if (selectedIds.length === 0) {
                Swal.fire("Error", "Please select at least one invoice.", "error");
                return;
            }

            Swal.fire({
                title: 'Are you sure?',
                text: 'Do you want to proceed with this supplier payment?',
                icon: 'warning',
                showCancelButton: true,
                confirmButtonText: 'Yes, proceed',
                cancelButtonText: 'Cancel'
            }).then((result) => {
                if (result.isConfirmed) {
                    const formData = new FormData();
                    formData.append('selectedIds', selectedIds);
                    formData.append('sulipper_id', this.sulipper_id);
                    formData.append('paymentAmount', this.paymentAmount);
                    formData.append('paymentMethod', this.paymentMethod);

                    const headers = {
                        'Content-Type': 'multipart/form-data'
                    };

                    axios.post('/api/post/updatePaymentStatusTravel', formData, { headers })
                        .then((res) => {
                            $(".loadingvideo").hide();
                            const modalElement = document.querySelector('.showtravelModal');
                            const modal = bootstrap.Modal.getInstance(modalElement);
                            if (modal) {
                                modal.hide();
                            }
                            this.sulipper_id = "";
                            this.paymentMethod = "";
                            this.paymentAmount = "";
                            this.selectedInvoicesTravel = [];
                            this.filterReport();
                            Swal.fire('Success', 'Payment recorded successfully.', 'success');
                        })
                        .catch((e) => {
                            this.notifmsg = e.response.data;
                            Swal.fire('Error', 'An error occurred during the payment.', 'error');
                        });
                }
            });
        },

        updatePaymentStatusMoneyTransfer() {

            const selectedIds = this.selectedInvoicesMoneyTransfer;
            if (selectedIds.length === 0) {
                Swal.fire("Error", "Please select at least one invoice.", "error");
                return;
            }

            Swal.fire({
                title: 'Are you sure?',
                text: 'Do you want to proceed with this supplier payment?',
                icon: 'warning',
                showCancelButton: true,
                confirmButtonText: 'Yes, proceed',
                cancelButtonText: 'Cancel'
            }).then((result) => {
                if (result.isConfirmed) {
                    const formData = new FormData();
                    formData.append('selectedIds', selectedIds);
                    formData.append('sulipper_id', this.sulipper_id);
                    formData.append('paymentAmount', this.paymentAmount);
                    formData.append('paymentMethod', this.paymentMethod);

                    const headers = {
                        'Content-Type': 'multipart/form-data'
                    };

                    axios.post('/api/post/updatePaymentStatusMoneyTransfer', formData, { headers })
                        .then((res) => {
                            $(".loadingvideo").hide();
                            const modalElement = document.querySelector('.showMoneyTransferModal');
                            const modal = bootstrap.Modal.getInstance(modalElement);
                            if (modal) {
                                modal.hide();
                            }
                            this.sulipper_id = "";
                            this.paymentMethod = "";
                            this.paymentAmount = "";
                            this.selectedInvoicesMoneyTransfer = [];
                            this.filterReport();
                            Swal.fire('Success', 'Payment recorded successfully.', 'success');
                        })
                        .catch((e) => {
                            this.notifmsg = e.response.data;
                            Swal.fire('Error', 'An error occurred during the payment.', 'error');
                        });
                }
            });


        },
        updatePaymentStatusOthers() {

            const selectedIds = this.selectedInvoicesOthers;
            if (selectedIds.length === 0) {
                Swal.fire("Error", "Please select at least one invoice.", "error");
                return;
            }

            Swal.fire({
                title: 'Are you sure?',
                text: 'Do you want to proceed with this supplier payment?',
                icon: 'warning',
                showCancelButton: true,
                confirmButtonText: 'Yes, proceed',
                cancelButtonText: 'Cancel'
            }).then((result) => {
                if (result.isConfirmed) {
                    const formData = new FormData();
                    formData.append('selectedIds', selectedIds);
                    formData.append('sulipper_id', this.sulipper_id);
                    formData.append('paymentAmount', this.paymentAmount);
                    formData.append('paymentMethod', this.paymentMethod);

                    const headers = {
                        'Content-Type': 'multipart/form-data'
                    };

                    axios.post('/api/post/updatePaymentStatusOthers', formData, { headers })
                        .then((res) => {
                            $(".loadingvideo").hide();
                            const modalElement = document.querySelector('.showMoneyOthersModal');
                            const modal = bootstrap.Modal.getInstance(modalElement);
                            if (modal) {
                                modal.hide();
                            }
                            this.sulipper_id = "";
                            this.paymentMethod = "";
                            this.paymentAmount = "";
                            this.selectedInvoicesMoneyTransfer = [];
                            this.filterReport();
                            Swal.fire('Success', 'Payment recorded successfully.', 'success');
                        })
                        .catch((e) => {
                            this.notifmsg = e.response.data;
                            Swal.fire('Error', 'An error occurred during the payment.', 'error');
                        });
                }
            });


        },

        updatePaymentStatusConsular() {

            const selectedIds = this.selectedInvoicesConsular;
            if (selectedIds.length === 0) {
                Swal.fire("Error", "Please select at least one invoice.", "error");
                return;
            }

            Swal.fire({
                title: 'Are you sure?',
                text: 'Do you want to proceed with this supplier payment?',
                icon: 'warning',
                showCancelButton: true,
                confirmButtonText: 'Yes, proceed',
                cancelButtonText: 'Cancel'
            }).then((result) => {
                if (result.isConfirmed) {
                    const formData = new FormData();
                    formData.append('selectedIds', selectedIds);
                    formData.append('sulipper_id', this.sulipper_id);
                    formData.append('paymentAmount', this.paymentAmount);
                    formData.append('paymentMethod', this.paymentMethod);

                    const headers = {
                        'Content-Type': 'multipart/form-data'
                    };

                    axios.post('/api/post/updatePaymentStatusConsular', formData, { headers })
                        .then((res) => {
                            $(".loadingvideo").hide();
                            const modalElement = document.querySelector('.showMoneyConsularModal');
                            const modal = bootstrap.Modal.getInstance(modalElement);
                            if (modal) {
                                modal.hide();
                            }
                            this.sulipper_id = "";
                            this.paymentMethod = "";
                            this.paymentAmount = "";
                            this.selectedInvoicesMoneyTransfer = [];
                            this.filterReport();
                            Swal.fire('Success', 'Payment recorded successfully.', 'success');
                        })
                        .catch((e) => {
                            this.notifmsg = e.response.data;
                            Swal.fire('Error', 'An error occurred during the payment.', 'error');
                        });
                }
            });


        },



        searchDataTravel() {
            $(".loadingvideo").show();
            $(".paytment_btn").hide();
            const formData = new FormData();
            let frm_date = $(".frm_date").val();
            let to_date = $(".to_date").val();
            let customer_id = $(".customer_id").val();
            formData.append('frm_date', frm_date);
            formData.append('to_date', to_date);
            formData.append('customer_id', customer_id);
            formData.append('sulipper_id', this.sulipper_id);
            const headers = {
                'Content-Type': 'multipart/form-data'
            };
            axios.post('/api/post/dueReportForTravel',
                formData, {
                headers
            }).then((res) => {
                $(".loadingvideo").hide();
                $(".paytment_btn").show();
                //    console.log(res.data.data);
                this.report_travel = res.data.data;
                this.total_due_travel = res.data.total_sum;
                // console.log(res.data.total_sum);
                this.frm_date = frm_date;
                this.to_date = to_date;
            }).catch(e => {
                this.notifmsg = e.response.data
            });
        },
        searchDataForMoney() {
            $(".loadingvideo").show();
            $(".paytment_btn").hide();
            const formData = new FormData();
            let frm_date = $(".frm_date").val();
            let to_date = $(".to_date").val();
            let customer_id = $(".customer_id").val();
            formData.append('frm_date', frm_date);
            formData.append('to_date', to_date);
            formData.append('customer_id', customer_id);
            formData.append('sulipper_id', this.sulipper_id);
            const headers = {
                'Content-Type': 'multipart/form-data'
            };
            axios.post('/api/post/dueReportMoney',
                formData, {
                headers
            }).then((res) => {
                $(".loadingvideo").hide();
                $(".paytment_btn").show();
                //    console.log(res.data.data);



                this.money_report = res.data.data;


               let acc = { due_amount: 0 };

this.money_report.forEach(data => {
  const recevAmount = parseFloat(data.receiving_amount) || 0;
  const rates = parseFloat(data.rate) || 1; // avoid divide-by-zero
  const custDeposit = parseFloat(data.customer_deposit) || 0;

  const forduecal = recevAmount / rates - custDeposit;

  if (!isNaN(forduecal) && forduecal >= 0) {
    acc.due_amount += forduecal;
  }
});

this.money_total_due = acc.due_amount.toFixed(2);


                //  console.log(res.data.total_sum);
                this.frm_date = frm_date;
                this.to_date = to_date;
            }).catch(e => {
                this.notifmsg = e.response.data
            });
        },


        searchDataConsular() {
            $(".loadingvideo").show();
            $(".paytment_btn").hide();
            const formData = new FormData();
            let frm_date = $(".frm_date").val();
            let to_date = $(".to_date").val();
            let customer_id = $(".customer_id").val();
            formData.append('frm_date', frm_date);
            formData.append('to_date', to_date);
            formData.append('customer_id', customer_id);
            formData.append('sulipper_id', this.sulipper_id);
            const headers = {
                'Content-Type': 'multipart/form-data'
            };
            axios.post('/api/post/deReportConsular',
                formData, {
                headers
            }).then((res) => {
                $(".loadingvideo").hide();
                $(".paytment_btn").show();
                //    console.log(res.data.data);
                this.consular_report = res.data.data;
                this.consular_total_due = res.data.total_sum;
                console.log(res.data.total_sum);
                this.frm_date = frm_date;
                this.to_date = to_date;
            }).catch(e => {
                this.notifmsg = e.response.data
            });
        },
        searchDataOthers() {
            $(".loadingvideo").show();
            $(".paytment_btn").hide();
            const formData = new FormData();
            let frm_date = $(".frm_date").val();
            let to_date = $(".to_date").val();
            let customer_id = $(".customer_id").val();
            formData.append('frm_date', frm_date);
            formData.append('to_date', to_date);
            formData.append('customer_id', customer_id);
            formData.append('sulipper_id', this.sulipper_id);
            const headers = {
                'Content-Type': 'multipart/form-data'
            };
            axios.post('/api/post/deReportOthers',
                formData, {
                headers
            }).then((res) => {
                $(".loadingvideo").hide();
                $(".paytment_btn").show();
                //    console.log(res.data.data);
                this.others_report = res.data.data;
                this.other_total_due = res.data.total_sum;
                console.log(res.data.total_sum);
                this.frm_date = frm_date;
                this.to_date = to_date;
            }).catch(e => {
                this.notifmsg = e.response.data
            });
        },
        onAmountInput(event) {
            let value = event.target.value;
            value = value.replace(/[^0-9.]/g, '');
            const parts = value.split('.');
            if (parts.length > 2) {
                value = parts[0] + '.' + parts.slice(1).join('');
            }
            this.paymentAmount = value;
        }
    }
};
</script>
<style>
table {
    border-collapse: collapse;
}

.report-table {
    width: 100%;
    border-collapse: collapse;
    font-family: Arial, sans-serif;
    font-size: 13px;
}

.report-table th,
.report-table td {
    border: 1px solid #ccc;
    padding: 2px;
    text-align: left;
}

.report-table thead {
    background-color: #f4f4f4;
    font-weight: bold;
}

.report-table tbody tr:nth-child(even) {
    background-color: #fafafa;
}

.report-table tbody tr:hover {
    background-color: #f1f1f1;
}

table,
td,
th {
    border: none;
    /* removes all borders */
    padding: 0;
    /* optional: remove cell padding */
}

table {
    border-collapse: collapse;
}

table,
th,
td {
    border: 1px solid rgb(68, 68, 68);
}
</style>