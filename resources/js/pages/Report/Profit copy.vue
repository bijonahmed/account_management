<template>
    <div class="main-wrapper">
        <div class="page-wrapper">
            <Nav />
            <div class="page-content">
                <div class="alert-primary" role="alert" style="margin-bottom:10px;">
                    <div class="row">
                        <div class="col-md-8">Report</div>
                    </div>
                </div>
                <ul class="nav nav-pills mb-3" id="pills-tab" role="tablist">
                    <li class="nav-item" role="presentation">
                        <button class="nav-link active" id="pills-home-tab" data-bs-toggle="pill"
                            data-bs-target="#pills-home" type="button" role="tab" aria-controls="pills-home"
                            aria-selected="true">Travel</button>
                    </li>
                    <li class="nav-item" role="presentation">
                        <button class="nav-link" id="pills-profile-tab" data-bs-toggle="pill"
                            data-bs-target="#pills-profile" type="button" role="tab" aria-controls="pills-profile"
                            aria-selected="false">Money Transfer</button>
                    </li>
                    <li class="nav-item" role="presentation">
                        <button class="nav-link" id="pills-profile-1-tab" data-bs-toggle="pill"
                            data-bs-target="#pills-profile-1" type="button" role="tab" aria-controls="pills-profile-1"
                            aria-selected="false">Othres Invoice</button>
                    </li>
                </ul>
                <div class="tab-content" id="pills-tabContent">
                    <div class="tab-pane fade show active" id="pills-home" role="tabpanel"
                        aria-labelledby="pills-home-tab">
                        <form @submit.prevent="SearchData()" id="formrest" class="forms-sample"
                            enctype="multipart/form-data">
                            <div class="row">
                                <div class="col-md-3">
                                    <div class="input-group mb-3">
                                        <select class="form-select company_id">
                                            <option value="">Select a Company...</option>
                                            <option v-for="(data, index) in companyList" :key="index" :value="data.id">
                                                {{ data.company_name }}
                                            </option>
                                        </select>
                                    </div>
                                </div>



                                <div class="col-md-3">
                                    <div class="input-group mb-3">
                                        <input type="date" class="form-control frm_date" v-model="frm_date"
                                            placeholder="From Date">
                                    </div>
                                </div>
                                <div class="col-md-3">
                                    <div class="input-group mb-3">
                                        <!-- <Datepicker class="form-control to_date" placeholder="To Date"></Datepicker>   -->
                                        <input type="date" class="form-control to_date" placeholder="To Date"
                                            v-model="to_date">
                                    </div>
                                </div>
                                <div class="col-md-6 d-none">
                                    <div class="input-group mb-3">
                                        <select class='form-control form-select customer_id' v-model="customer_id">
                                            <option value=''>Select Customer</option>
                                            <option v-for='data in customers' :value='data.customer_id' :key="data.id">
                                                {{ data.name }}</option>
                                        </select>
                                    </div>
                                </div>
                                <div class="col-md-2">
                                    <button type="submit" class="btn btn-primary w-100">Submit</button>
                                </div>
                            </div>
                        </form>
                        <div class="d-flex justify-content-center">
                            <div class="spinner-border loadingvideo" role="status" style="display:none">
                                <span class="visually-hidden">Loading...</span>
                            </div>
                        </div>
                        <hr>

                        <!-- start  -->
                        <div class="table-responsive">
                            <table border="2" cellpadding="1" cellspacing="2" width="100%" class="table">
                                <td height="17" width="107">
                                    <div align="center" class="style8">Transaction Date&nbsp;</div>
                                </td>
                                <td width="110">
                                    <div align="center" class="style8">PNR&nbsp;</div>
                                </td>
                                <td width="110">
                                    <div align="center" class="style8">Passesnger Name&nbsp;</div>
                                </td>
                                <td width="116">
                                    <div align="center" class="style8">Route</div>
                                </td>
                                <td width="123">
                                    <div align="center" class="style8">Net Amount/Supplier Amount</div>
                                </td>
                                <td width="98">
                                    <div align="center" class="style8">Customer Amount&nbsp;</div>
                                </td>
                                <td width="58">
                                    <div align="center" class="style8">Profit</div>
                                </td>
                                <td width="66">
                                    <div align="center" class="style8">Amount Paid</div>
                                </td>
                                <td width="56">
                                    <div align="center" class="style8">Due</div>
                                </td>
                                <td width="99">
                                    <div align="center" class="style8">Suppliers</div>
                                </td>
                                <td width="123">
                                    <div align="center" class="style8">Payment type&nbsp;</div>
                                </td>

                                <tr height="17" v-for="data in report" :key="data.inv_id">
                                    <td height="17" class="style8"><span class="style21">{{ data.created_at }}</span>
                                    </td>
                                    <td class="style8">{{ data.vendorpnr }}</td>
                                    <td class="style8"><span class="style21">{{ data.customer_name }}</span></td>
                                    <td class="style8"><span class="style21">{{ data.depart_to }}</span></td>
                                    <td class="style8"><span class="style21 text-center">
                                            <center>{{ data.net_amount }}</center>
                                        </span></td>
                                    <td class="style8"><span class="style21">
                                            <center>{{ data.customer_amount }}</center>
                                        </span></td>
                                    <td class="style8"><span class="style21">
                                            <center>{{ data.profit }}</center>
                                        </span></td>
                                    <td class="style8"><span class="style21">
                                            <center>{{ data.customer_deposit }}</center>
                                        </span></td>
                                    <td class="style8"><span class="style21">
                                            <center>{{ data.due_amount }}</center>
                                        </span></td>
                                    <td class="style8"><span class="style21">{{ data.supplier_name }}</span></td>
                                    <td class="style8"><span class="style21">
                                            <p v-if="data.status === 1">Bank</p>
                                            <p v-else-if="data.status === 2">Bank Transfer</p>
                                            <p v-else-if="data.status === 3">Cash</p>
                                            <p v-else>Unknown Status</p>
                                        </span></td>
                                </tr>

                            </table>
                        </div>

                        <!-- end -->

                    </div>

                    <div class="tab-pane fade" id="pills-profile" role="tabpanel" aria-labelledby="pills-profile-tab">
                        <form @submit.prevent="SearchDataMoney()" id="formrest" class="forms-sample"
                            enctype="multipart/form-data">
                            <div class="row">
                                <div class="col-md-3">
                                    <div class="input-group mb-3">
                                        <select class="form-select company_id">
                                            <option value="">Select a Company...</option>
                                            <option v-for="(data, index) in companyList" :key="index" :value="data.id">
                                                {{ data.company_name }}
                                            </option>
                                        </select>
                                    </div>
                                </div>
                                <div class="col-md-3">
                                    <div class="input-group mb-3">
                                        <input type="date" class="form-control frm_date" id="money_frm_date"
                                            placeholder="From Date">
                                    </div>
                                </div>
                                <div class="col-md-3">
                                    <div class="input-group mb-3">
                                        <!-- <Datepicker class="form-control to_date" placeholder="To Date"></Datepicker>   -->
                                        <input type="date" class="form-control to_date" id="money_to_date"
                                            placeholder="To Date">
                                    </div>
                                </div>
                                <div class="col-md-3 d-none">
                                    <div class="input-group mb-3">
                                        <select class='form-control form-select customer_id' v-model="customer_id">
                                            <option value=''>Select Customer</option>
                                            <option v-for='data in customers' :value='data.customer_id'
                                                :key="data.customer_id">{{ data.name }}</option>
                                        </select>
                                    </div>
                                </div>
                                <div class="col-md-3">
                                    <button type="submit" class="btn btn-primary w-100">Submit</button>
                                </div>
                            </div>
                        </form>
                        <div class="d-flex justify-content-center">
                            <div class="spinner-border loadingvideo" role="status" style="display:none">
                                <span class="visually-hidden">Loading...</span>
                            </div>
                        </div>
                        <hr>
                        <center>
                            <div style="display: inline-flex; gap: 10px;">
                                <button @click="downloadExcel" class="btn btn-success">
                                    <i class="fas fa-file-excel"></i> Download XLSX
                                </button>
                                <button @click="printReport" class="btn btn-primary">
                                    <i class="fas fa-print"></i> Print
                                </button>
                            </div>
                        </center>
                        <br />
                        <!-- start money  -->
                        <div id="report-table">
                            <table class="report-table" style="border-collapse: collapse; width: 100%;">
                                <thead>
                                    <tr>
                                        <th class="text-end">Tran. Date</th>
                                        <th>Beneficiary Name</th>
                                        <th>Sender Name</th>
                                        <th class="text-end">Sending Amount</th>
                                        <th class="text-end">Rate</th>
                                        <th class="text-end">Sale</th>
                                        <th class="text-end">Charge</th>
                                        <th class="text-end">Others Charge</th>
                                        <th class="text-end">Total Sale</th>
                                        <th class="text-end">Profit</th>
                                        <th class="text-end">Customer Paid</th>
                                        <th class="text-end">Payment Type</th>
                                        <th class="text-end">Due</th>
                                        <th class="text-end">Supplier</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="data in money_report" :key="data.mone_transfer_id">
                                        <td class="text-end">{{ data.invoice_date }}</td>
                                        <td class="text-start">{{ data.beneficiary_name }}</td>
                                        <td class="text-start">{{ data.customer_name }}</td>
                                        <td class="text-end">{{ parseFloat(data.receiving_amount).toFixed(2) }}</td>
                                        <td class="text-end">{{ parseFloat(data.rate).toFixed(2) }}</td>
                                        <td class="text-end">{{ (data.receiving_amount / data.rate).toFixed(2) }}</td>
                                        <td class="text-end">{{ parseFloat(data.fees).toFixed(2) }}</td>
                                        <td class="text-end">{{ parseFloat(data.others_fees).toFixed(2) }}</td>
                                        <td class="text-end"><b>{{ formattedTotal(data) }}</b>
                                        </td>
                                        <td class="text-end">{{ parseFloat(data.profit).toFixed(2) }}</td>
                                        <td class="text-end">{{ parseFloat(data.customer_deposit).toFixed(2) }}</td>
                                        <td class="text-end">
                                            <span v-if="data.status === 1">Bank</span>
                                            <span v-else-if="data.status === 2">Bank Transfer</span>
                                            <span v-else-if="data.status === 3">Cash</span>
                                            <span v-else>Unknown</span>
                                        </td>
                                        <td class="text-end">{{ parseFloat(data.due_amount).toFixed(2) }}</td>
                                        <td><small>{{ data.supplier_name }}</small></td>
                                    </tr>
                                    <tr>
                                        <td class="text-end" style="color: gray;"></td>
                                        <td class="text-end" style="color: gray;"></td>
                                        <td class="text-end" style="color: gray;"></td>

                                        <td class="text-end" style="color: blue;"><b>{{
                                            totals.receiving_amount.toFixed(2) }}</b></td>
                                        <td class="text-end" style="color: indigo;"><b>{{ totals.rate.toFixed(2) }}</b>
                                        </td>
                                        <td class="text-end" style="color: green;"><b>{{
                                            totals.receiving_divided_by_rate.toFixed(2) }}</b></td>
                                        <td class="text-end" style="color: deeppink;"><b>{{ totals.fees.toFixed(2) }}</b></td>
                                        <td class="text-end" style="color: goldenrod;"><b>{{
                                            totals.others_fees.toFixed(2) }}</b></td>
                                        <td class="text-end" style="color: purple;">
                                            <b>{{ totals.total_calculated.toFixed(2) }}</b>

                                        </td>
                                        <td class="text-end" style="color: red;"><b>{{ totals.profit.toFixed(2) }}</b>
                                        </td>
                                        <td class="text-end" style="color: teal;"><b>{{
                                            totals.customer_deposit.toFixed(2) }}</b></td>

                                        <td></td>

                                        <td class="text-end" style="color: orange;"><b>{{ totals.due_amount.toFixed(2)
                                                }}</b></td>
                                        <td></td>
                                    </tr>
                                    <tr class="bgtrColor">
                                        <td class="txttrcolor">
                                            <center>-</center>
                                        </td>
                                        <td class="txttrcolor">
                                            <center>-</center>
                                        </td>
                                        <td class="txttrcolor">
                                            <center>-</center>
                                        </td>
                                        <td class="txttrcolor">
                                            <center>-</center>
                                        </td>
                                        <td class="txttrcolor">
                                            <center>-</center>
                                        </td>
                                        <td class="txttrcolor">
                                            <center>-</center>
                                        </td>
                                        <td class="txttrcolor">
                                            <center>-</center>
                                        </td>
                                        <td class="txttrcolor">
                                            <center>-</center>
                                        </td>
                                        <td class="txttrcolor">
                                            <center>-</center>
                                        </td>
                                        <td class="txttrcolor">
                                            <center>-</center>
                                        </td>
                                        <td class="txttrcolor">
                                            <center>-</center>
                                        </td>
                                        <td class="txttrcolor">
                                            <center>-</center>
                                        </td>
                                        <td class="txttrcolor">
                                            <center>-</center>
                                        </td>
                                        <td class="txttrcolor">
                                            <center>-</center>
                                        </td>
                                    </tr>
                                    <tr class="bgtrColor">
                                        <td colspan="3" class="text-end">Total No of transaction</td>
                                        <td class="text-end"><b>{{ totalTransaction }}</b></td>
                                        <td colspan="10"></td>
                                    </tr>

                                    <tr class="bgtrColor">
                                        <td colspan="3" class="text-end">Total Amount of (Sales)</td>
                                        <td class="text-end"><b>
                                            <!-- {{ Number(totals?.receiving_amount || 0).toFixed(2) }} /
                                                {{ Number(totals?.rate || 0).toFixed(2) }} = -->

                                                {{totals.receiving_divided_by_rate.toFixed(2)}}
                                                    
                                                </b></td>
                                        <td colspan="10"></td>
                                    </tr>


                                    <tr class="bgtrColor">
                                        <td colspan="3" class="text-end">Total Amount of Charges</td>
                                        <td class="text-end"><b>{{ totals.fees.toFixed(2) }}</b></td>
                                        <td colspan="10"></td>
                                    </tr>

                                    <tr class="bgtrColor">
                                        <td colspan="3" class="text-end">Total Amount of Other Charges</td>
                                        <td class="text-end"><b>{{ totals.others_fees.toFixed(2) }}</b></td>
                                        <td colspan="10"></td>
                                    </tr>

                                    <tr class="bgtrColor">
                                        <td colspan="3" class="text-end">Total Amount of (Total Sales)</td>
                                        <td class="text-end"><b> 
                                                <!-- {{ totals.receiving_divided_by_rate.toFixed(2) }} +
                                                {{ totals.fees.toFixed(2) }} +
                                                {{ totals.others_fees.toFixed(2) }} = -->

                                                {{ (
                                                    totals.receiving_divided_by_rate +
                                                    totals.fees +
                                                    totals.others_fees
                                                ).toFixed(2) }}</b>
                                        </td>
                                        <td colspan="10"></td>
                                    </tr>
                                    <tr class="bgtrColor">
                                        <td colspan="3" class="text-end">Total Profit</td>
                                        <td class="text-end"><b>{{ money_total_profit }}</b></td>
                                        <td colspan="10"></td>
                                    </tr>

                                    <tr class="bgtrColor">
                                        <td colspan="3" class="text-end">Total Outstanding / Due</td>
                                        <td class="text-end"><b>{{ total_due }}</b></td>
                                        <td colspan="10"></td>
                                    </tr>

                                </tbody>
                            </table>
                            <table class="report-table" style="border-collapse: collapse; width: 100%; display: none;">
                                <tr>
                                    <td width="24%">Total Number of Transaction </td>
                                    <td width="1%">&nbsp;:</td>
                                    <td width="75%"><strong>&nbsp;<b>{{ totalTransaction }}</b></strong></td>
                                </tr>
                                <tr>
                                    <td>Transaction Balance</td>
                                    <td>&nbsp;:</td>
                                    <td><strong>&nbsp;<b>{{ total_fees }}</b></strong></td>
                                </tr>
                                <tr>
                                    <td>Total Profit </td>
                                    <td>&nbsp;:</td>
                                    <td><strong>&nbsp;<b>{{ money_total_profit }}</b></strong></td>
                                </tr>
                                <tr>
                                    <td>Total Due /Outstanding </td>
                                    <td>&nbsp;:</td>
                                    <td><strong>&nbsp;<b>{{ total_due }}</b></strong></td>
                                </tr>
                            </table>

                            <!-- end money -->
                            <div class="row">
                                <br>

                                <center><span style="font-size: 25px; font-weight: bold;color:green;">Total
                                        Profit:&nbsp;{{ money_total_profit }}</span></center>

                            </div>

                        </div>

                    </div>

                    <div class="tab-pane fade" id="pills-profile-1" role="tabpanel" aria-labelledby="pills-profile-tab">
                        <form @submit.prevent="SearchDataOthers()" id="formrest" class="forms-sample"
                            enctype="multipart/form-data">
                            <div class="row">

                                <div class="col-md-3">
                                    <div class="input-group mb-3">
                                        <select class="form-select company_id">
                                            <option value="">Select a Company...</option>
                                            <option v-for="(data, index) in companyList" :key="index" :value="data.id">
                                                {{ data.company_name }}
                                            </option>
                                        </select>
                                    </div>
                                </div>


                                <div class="col-md-3">
                                    <div class="input-group mb-3">
                                        <input type="date" class="form-control frm_date" id="o_frm_date"
                                            placeholder="From Date">
                                    </div>
                                </div>
                                <div class="col-md-3">
                                    <div class="input-group mb-3">
                                        <!-- <Datepicker class="form-control to_date" placeholder="To Date"></Datepicker>   -->
                                        <input type="date" class="form-control to_date" id="o_to_date"
                                            placeholder="To Date">
                                    </div>
                                </div>
                                <div class="col-md-3 d-none">
                                    <div class="input-group mb-3">
                                        <select class='form-control form-select customer_id' v-model="customer_id">
                                            <option value=''>Select Customer</option>
                                            <option v-for='data in customers' :value='data.customer_id'
                                                :key="data.customer_id">{{ data.name }}
                                            </option>
                                        </select>
                                    </div>
                                </div>
                                <div class="col-md-2">
                                    <button type="submit" class="btn btn-primary w-100">Submit</button>
                                </div>
                            </div>
                        </form>
                        <div class="d-flex justify-content-center">
                            <div class="spinner-border loadingvideo" role="status" style="display:none">
                                <span class="visually-hidden">Loading...</span>
                            </div>
                        </div>
                        <hr>
                        <table id="datatable" class="table table-striped table-hover table-sm">
                            <thead>
                                <tr>
                                    <!-- <th>Invoice ID </th> -->
                                    <th>Datetime</th>
                                    <th>Customer Name</th>
                                    <th>Purpose</th>
                                    <th>Net Amount</th>
                                    <th>Customer Amount</th>
                                    <th>Amount Paid</th>
                                    <!-- <th>Amount Due</th> -->
                                    <th>Create By</th>
                                    <th>Profit Amount</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for='data in others_report'>
                                    <!-- <td>{{ data.others_inv_id  }}</td> -->
                                    <td>{{ data.create_date }}</td>
                                    <td>{{ data.customer_name }}</td>
                                    <td>{{ data.purpose }}</td>
                                    <td>{{ data.net_amount }}</td>
                                    <td>{{ data.customer_amount }}</td>
                                    <td>{{ data.amount_paid }}</td>
                                    <!-- <td>{{ data.due_amount }}</td> -->
                                    <td>{{ data.name }}</td>
                                    <td>{{ data.profit }}</td>
                                </tr>
                            </tbody>
                        </table>
                        <div class="row">
                            <div class="col-8"></div>
                            <div class="col-3">
                                <center><span style="font-size: 25px; font-weight: bold;color:green;">Total Profit: {{
                                    others_total_profit }}</span></center>
                            </div>
                        </div>
                    </div>

                </div>
                <!-- row -->
            </div>
            <!-- Button trigger modal -->
            <Footer />
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
            videoList: [],
            customers: [],
            frm_date: '06-01-2024',
            to_date: '06-30-2024',
            total_fees: 0,
            total_profit: 0,
            totalTransaction: 0,
            total_due: 0,
            money_total_profit: 0,
            others_total_profit: 0,
            customer_id: '',
            report: [],
            money_report: [],
            others_report: [],
            companyList: [],
        };
    },
    computed: {
        totals() {
            return this.money_report.reduce((acc, data) => {
                const receivingAmount = parseFloat(data.receiving_amount) || 0;
                const rate = parseFloat(data.rate) || 1; // Avoid divide by zero
                const fees = parseFloat(data.fees) || 0;
                const othersFees = parseFloat(data.others_fees) || 0;
                const profit = parseFloat(data.profit) || 0;
                const customerDeposit = parseFloat(data.customer_deposit) || 0;
                const dueAmount = parseFloat(data.due_amount) || 0;

                const sale = receivingAmount / rate;
                const total = sale + fees + othersFees;

                acc.receiving_amount += receivingAmount;
                acc.rate += rate;
                acc.receiving_divided_by_rate += sale;
                acc.fees += fees;
                acc.others_fees += othersFees;
                acc.total_calculated += total;
                acc.profit += profit;
                acc.customer_deposit += customerDeposit;
                acc.due_amount += dueAmount;

                return acc;
            }, {
                receiving_amount: 0,
                rate: 0,
                receiving_divided_by_rate: 0,
                fees: 0,
                others_fees: 0,
                total_calculated: 0,
                profit: 0,
                customer_deposit: 0,
                due_amount: 0
            });
        },

    },

    mounted() {
        this.getCompanyList();
        //   this.customerlist();
        //  $('.frm_date,.to_date').datepicker();
    },
    methods: {
        formattedTotal(data) {
            const receivingAmount = parseFloat(data.receiving_amount) || 0;
            const rate = parseFloat(data.rate) || 1; // Avoid divide by zero
            const fees = parseFloat(data.fees) || 0;
            const othersFees = parseFloat(data.others_fees) || 0;

            const sale = receivingAmount / rate;
            const total = sale + fees + othersFees;
            //console.log("Total =", total);
            return total.toFixed(2);
        },
        async printReport() {
            const printContents = document.getElementById('report-table').outerHTML;
            const style = `
        <style>
            table {
                width: 100%;
                border-collapse: collapse;
                font-family: Arial, sans-serif;
            }
            th, td {
                border: 1px solid #ccc;
                padding: 8px;
                text-align: right;
            }
            th {
                background-color: #f2f2f2;
            }
        </style>
    `;

            const printWindow = window.open('', '', 'height=600,width=900');
            printWindow.document.write('<html><head><title>Money Report</title>');
            printWindow.document.write(style);
            printWindow.document.write('</head><body>');
            printWindow.document.write('<h2>Money Report</h2>');
            printWindow.document.write(printContents);
            printWindow.document.write('</body></html>');
            printWindow.document.close();
            printWindow.focus();
            printWindow.print();
            printWindow.close();
        },

        async downloadExcel() {
            const workbook = new ExcelJS.Workbook();
            const worksheet = workbook.addWorksheet('Report');

            // Get the HTML table
            const table = document.getElementById('report-table');

            // Process table header
            const headerRow = [];
            table.querySelectorAll('thead tr th').forEach(th => {
                headerRow.push(th.innerText.trim());
            });
            worksheet.addRow(headerRow);

            // Process table body rows
            table.querySelectorAll('tbody tr').forEach(tr => {
                const rowData = [];
                tr.querySelectorAll('td').forEach(td => {
                    rowData.push(td.innerText.trim());
                });
                worksheet.addRow(rowData);
            });

            // Apply border style to all cells
            worksheet.eachRow(row => {
                row.eachCell(cell => {
                    cell.border = {
                        top: { style: 'thin' },
                        left: { style: 'thin' },
                        bottom: { style: 'thin' },
                        right: { style: 'thin' }
                    };
                });
            });

            // Adjust column widths based on max length of content
            worksheet.columns.forEach(column => {
                let maxLength = 10;
                column.eachCell({ includeEmpty: true }, cell => {
                    if (cell.value) {
                        maxLength = Math.max(maxLength, cell.value.toString().length);
                    }
                });
                column.width = maxLength + 2;
            });

            // Export file
            const buffer = await workbook.xlsx.writeBuffer();
            const blob = new Blob([buffer], { type: "application/octet-stream" });
            saveAs(blob, "money-report.xlsx");
        },
        calculateTotal(data) {
            console.log("Data:" + data);
            const receivingAmount = parseFloat(data.receiving_amount) || 0;
            const rate = parseFloat(data.rate) || 1; // prevent division by zero
            const fees = parseFloat(data.fees) || 0;
            const othersFees = parseFloat(data.others_fees) || 0;
            const sale = receivingAmount / rate;
            const total = sale + fees + othersFees;
            return total;
        },
        formatNumber(value) {
            const number = parseFloat(value);
            if (isNaN(number)) return '0.00';
            return number.toFixed(2);
        },
        getCompanyList() {
            axios.get('/api/company/getcountryList').then(response => {
                this.companyList = response.data.data;
            });
        },

        customerlist() {
            axios.get('/api/category/allCustomers').then(response => {
                this.customers = response.data.data;
            });
        },
        SearchData() {

            $(".loadingvideo").show();
            const formData = new FormData();
            let frm_date = $(".frm_date").val();
            let to_date = $(".to_date").val();
            let customer_id = $(".customer_id").val();
            let company_id = $(".company_id").val();
            formData.append('frm_date', frm_date);
            formData.append('to_date', to_date);
            formData.append('customer_id', customer_id);
            formData.append('company_id', company_id);

            const headers = {
                'Content-Type': 'multipart/form-data'
            };
            axios.post('/api/post/profitReport',
                formData, {
                headers
            }).then((res) => {
                $(".loadingvideo").hide();

                this.report = res.data.data;
                this.total_profit = res.data.total_sum;
                console.log(res.data.total_sum);
                this.frm_date = frm_date;
                this.to_date = to_date;
                this.company_id = company_id;
            }).catch(e => {
                this.notifmsg = e.response.data
            });
        },

        SearchDataMoney() {
            $(".loadingvideo").show();
            const formData = new FormData();
            let frm_date = $("#money_frm_date").val();
            let to_date = $("#money_to_date").val();
            let customer_id = $(".customer_id").val();
            let company_id = $(".company_id").val();
            formData.append('frm_date', frm_date);
            formData.append('to_date', to_date);
            formData.append('customer_id', customer_id);
            formData.append('company_id', company_id);
            const headers = {
                'Content-Type': 'multipart/form-data'
            };
            axios.post('/api/post/profitReportMoney',
                formData, {
                headers
            }).then((res) => {
                $(".loadingvideo").hide();
                //    console.log(res.data.data);
                this.money_report = res.data.data;
                this.money_total_profit = res.data.total_sum;
                this.total_fees = res.data.total_fees;
                // console.log(res.data.total_sum);
                this.total_due = res.data.total_due;
                this.totalTransaction = res.data.totalTransaction;
                this.frm_date = frm_date;
                this.to_date = to_date;
                this.company_id = company_id;
            }).catch(e => {
                this.notifmsg = e.response.data
            });
        },
        SearchDataOthers() {
            $(".loadingvideo").show();
            const formData = new FormData();
            let frm_date = $("#o_frm_date").val();
            let to_date = $("#o_to_date").val();
            let customer_id = $(".customer_id").val();
            let company_id = $(".company_id").val();

            formData.append('frm_date', frm_date);
            formData.append('to_date', to_date);
            formData.append('customer_id', customer_id);
            formData.append('company_id', company_id);
            const headers = {
                'Content-Type': 'multipart/form-data'
            };
            axios.post('/api/post/profitReportOthers',
                formData, {
                headers
            }).then((res) => {
                $(".loadingvideo").hide();
                //    console.log(res.data.data);
                this.others_report = res.data.data;
                this.others_total_profit = res.data.total_sum;
                console.log(res.data.total_sum);
                this.frm_date = frm_date;
                this.to_date = to_date;
                this.company_id = company_id;
            }).catch(e => {
                this.notifmsg = e.response.data
            });
        },
        calculateTotal() {
            const receivingAmount = parseFloat(this.money_report.receiving_amount) || 0;
            const fees = parseFloat(this.money_report.fees) || 0;
            const othersFees = parseFloat(this.money_report.others_fees) || 0;

            return (receivingAmount + fees + othersFees);
        }
    }
};
</script>

<style>
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

.style8 {
    font-size: 12px;
}

.style13 {
    font-size: 10
}

.style21 {
    font-size: 10;
    font-family: "Times New Roman", Times, serif;
}


.bgtrColor {
    background-color: #8583836b;
}

.txttrcolor {
    color: white;
}


/* Print start */

/* end */
</style>
