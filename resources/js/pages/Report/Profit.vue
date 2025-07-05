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
                <!-- Filter Area -->

                <form @submit.prevent="filterReport()" id="formrest" class="forms-sample" enctype="multipart/form-data">
                    <div class="row">
                        <div class="col-md-3">
                            <div class="input-group mb-3">
                                <select class="form-select company_id" v-model="selectedCompany"
                                    @change="fetchSuppliers">
                                    <option value="">All Company...</option>
                                    <option v-for="(data, index) in companyList" :key="index" :value="data.id">
                                        {{ data.company_name }}
                                    </option>
                                </select>
                            </div>
                        </div>

                        <div class="col-md-3" v-if="supplierList.length > 0">
                            <div class="input-group mb-3">
                                <select class="form-select sulipper_id" v-model="suplier_id">
                                    <option value="">All Supplier...</option>
                                    <option v-for="(data, index) in supplierList" :key="index"
                                        :value="data.sulipper_id">
                                        {{ data.name }}
                                    </option>
                                </select>
                            </div>
                        </div>
                        <div class="col-md-2">
                            <div class="input-group mb-3">
                                <input type="date" class="form-control frm_date" id="money_frm_date"
                                    placeholder="From Date" v-model="frm_date">
                            </div>
                        </div>
                        <div class="col-md-2">
                            <div class="input-group mb-3">

                                <input type="date" class="form-control to_date" id="money_to_date" placeholder="To Date"
                                    v-model="to_date">
                            </div>
                        </div>

                        <div class="col-md-2">
                            <button type="submit" class="btn btn-primary w-100">Submit</button>
                        </div>
                    </div>
                </form>
                <!-- END Filter Area -->



                <div class="d-flex justify-content-center">
                    <div class="spinner-border loadingvideo" role="status" style="display:none">
                        <span class="visually-hidden">Loading...</span>
                    </div>
                </div>




                <!-- For Travel -->

                <div class="for_travel">
                    <span><u>Travel Report: {{ firstCompanyName
                            }} </u></span>
                    <!-- Travel-Hamadan International Limited -->
                    <div class="table-responsive">
                        <table class="report-table" style="border-collapse: collapse; width: 100%;">
                            <thead>
                                <tr>
                                    <th class="text-center">Transaction Date</th>
                                    <th class="text-center">PNR</th>
                                    <th class="text-center">Passenger Name</th>
                                    <th class="text-center">Route</th>
                                    <th class="text-center">Net Amount / Supplier Amount</th>
                                    <th class="text-center">Customer Amount</th>
                                    <th class="text-center">Profit</th>
                                    <th class="text-center">Amount Paid</th>
                                    <th class="text-center">Due</th>
                                    <th class="text-center">Suppliers</th>
                                    <th class="text-center">Payment Type</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="data in report" :key="data.inv_id">
                                    <td class="text-center">{{ data.created_at }}</td>
                                    <td class="text-center">{{ data.vendorpnr }}</td>
                                    <td class="text-end">{{ data.customer_name }}</td>
                                    <td class="text-center">{{ data.depart_to }}</td>
                                    <td class="text-end">{{ data.net_amount }}</td>
                                    <td class="text-end">{{ data.customer_amount }}</td>
                                    <td class="text-end">{{ data.profit }}</td>
                                    <td class="text-end">{{ data.customer_deposit }}</td>
                                    <td class="text-end">{{ data.due_amount }}</td>
                                    <td class="text-center">{{ data.supplier_name }}</td>
                                    <td class="text-center">
                                        <span v-if="data.status === 1">Bank</span>
                                        <span v-else-if="data.status === 2">Bank Transfer</span>
                                        <span v-else-if="data.status === 3">Cash</span>
                                        <span v-else>Unknown Status</span>
                                    </td>
                                </tr>
                            </tbody>
                        </table>

                    </div>
                    <div class="row text-end">
                        <span style="font-weight: bold;color:green;">Total Profit: {{ totalProfit }}</span>
                    </div>

                    <div v-if="selectedCompany == 1">
                        Total Amount of Sale: {{ travel_total_amount_of_Sale }}<br />
                        Total Net Amount : {{ travel_total_net_amount }}<br />
                        Total Profit : {{ travel_total_profit }}<br />
                        Total Due : {{ travel_total_due }}<br />
                    </div>


                </div>
                <!-- END Travel -->

                <!-- Money Transfer -->

                <div class="for_moneytransfer">
                    <span><u>Money Transfer Report: {{
                        secondCompanyName }} </u></span>
                    <!-- Hamadan Express Limited -->
                    <!-- <center>
                        <div style="display: inline-flex; gap: 10px;">
                            <button @click="downloadExcel" class="btn btn-success">
                                <i class="fas fa-file-excel"></i> Download XLSX
                            </button>
                            <button @click="printReport" class="btn btn-primary">
                                <i class="fas fa-print"></i> Print
                            </button>
                        </div>
                    </center> -->
                    <br />
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
                                    <td class="text-end">
                                        {{ (formattedTotal(data) - data.customer_deposit).toFixed(2) }}
                                        <!-- --- {{ (data.receiving_amount / data.rate - data.customer_deposit).toFixed(2) }} -->
                                        <!-- {{ parseFloat(data.due_amount) }} -->
                                    </td>
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
                                    <td class="text-end" style="color: deeppink;"><b>{{ totals.fees.toFixed(2)
                                            }}</b></td>
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

                                    <td class="text-end" style="color: orange;">
                                        {{ totalMoneyDueAmount }}
                                        <!-- <b>{{ totals.due_amount.toFixed(2) }}</b> -->

                                    </td>
                                    <td></td>
                                </tr>

                                <tr class="bgtrColor d-none">
                                    <td colspan="3" class="text-end">Total No of transaction</td>
                                    <td class="text-end"><b>{{ totalTransaction }}</b></td>
                                    <td colspan="10"></td>
                                </tr>

                                <tr class="bgtrColor d-none">
                                    <td colspan="3" class="text-end">Total Amount of (Sales)</td>
                                    <td class="text-end"><b>
                                            <!-- {{ Number(totals?.receiving_amount || 0).toFixed(2) }} /
                                                {{ Number(totals?.rate || 0).toFixed(2) }} = -->

                                            {{ totals.receiving_divided_by_rate.toFixed(2) }}

                                        </b></td>
                                    <td colspan="10"></td>
                                </tr>


                                <tr class="bgtrColor d-none">
                                    <td colspan="3" class="text-end">Total Amount of Charges</td>
                                    <td class="text-end"><b>{{ totals.fees.toFixed(2) }}</b></td>
                                    <td colspan="10"></td>
                                </tr>

                                <tr class="bgtrColor d-none">
                                    <td colspan="3" class="text-end">Total Amount of Other Charges</td>
                                    <td class="text-end"><b>{{ totals.others_fees.toFixed(2) }}</b></td>
                                    <td colspan="10"></td>
                                </tr>

                                <tr class="bgtrColor d-none">
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
                                <tr class="bgtrColor d-none">
                                    <td colspan="3" class="text-end">Total Profit</td>
                                    <td class="text-end"><b>{{ money_total_profit }}</b></td>
                                    <td colspan="10"></td>
                                </tr>

                                <tr class="bgtrColor d-none">
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
                        <div class="row text-end">
                            <br>

                            <span style="font-weight: bold;color:green;">Total Profit:&nbsp;{{ money_total_profit }}</span>

                        </div>

                        <div v-if="selectedCompany == 2">
                            Total Sale: {{ totals.receiving_divided_by_rate.toFixed(2) }}<br>
                            Net/Supplier Amount: {{ money_net_supplier_amnt }}<br>
                            Total Due: {{ totals.due_amount.toFixed(2) }}<br>
                            Total Profit : {{ money_total_profit }}<br>

                        </div>

                    </div>

                </div>
                <!-- END Money Transfer -->

                <!-- Consular Report -->
                <div class="for_consular">
                    <span><u>Consular Report: {{
                        thirdCompanyName }}</u></span>
                    <!-- Hamadan Consular Services  -->
                    <div class="table-responsive">
                        <table class="report-table" style="border-collapse: collapse; width: 100%;">
                            <thead>
                                <tr>
                                    <!-- <th>Invoice ID</th> -->
                                    <th>Datetime</th>
                                    <th>Customer Name</th>
                                    <th>Purpose</th>
                                    <th class="text-center">Net Amount</th>
                                    <th class="text-center">Customer Amount</th>
                                    <th class="text-center">Amount Paid</th>
                                    <th class="text-end">Amount Due</th>
                                    <th class="text-center">Profit Amount</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="data in consular_report" :key="data.consular_inv_id">
                                    <!-- <td>{{ data.others_inv_id }}</td> -->
                                    <td>{{ data.create_date }}</td>
                                    <td>{{ data.customer_name }}</td>
                                    <td>{{ data.purpose }}</td>
                                    <td class="text-end">{{ data.net_amount }}</td>
                                    <td class="text-end">{{ data.customer_amount }}</td>
                                    <td class="text-end">{{ data.amount_paid }}</td>
                                    <td class="text-end">{{ data.due_amount }}</td>

                                    <td class="text-end">{{ data.profit }}</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                    <div class="row text-end">
                        <span style="font-weight: bold;color:green;">Total Profit: {{ consular_total_profit }}</span>
                    </div>
                    <div v-if="selectedCompany == 3">
                        Total amount: {{ consular_total_amt }} <br />
                        Total due: {{ consular_total_due_amt }} <br />
                        Profit :{{ consular_total_profit }}<br />

                    </div>

                </div>
                <!-- END Consular -->

                <!-- Corporate Report -->
                <div class="for_others">
                    <span><u>Others Report: {{
                        fourthCompanyName }}</u></span>
                    <!-- Hamadan Corporate Services  -->
                    <div class="table-responsive">
                        <table class="report-table" style="border-collapse: collapse; width: 100%;">
                            <thead>
                                <tr>
                                    <th>Invoice No</th>
                                    <th>Invoice Date</th>
                                    <th>Supplier</th>
                                    <th>Customer</th>
                                    <th class="text-center">Amount</th>
                                    <th class="text-center">Total Amount</th>
                                    <th class="text-center">Amount Paid</th>
                                    <th class="text-center">Charge/Fee</th>
                                    <th class="text-center">Amount Remaining/Due</th>
                                    <!-- <th>Create By</th> -->
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="data in others_report" :key="data.others_inv_id">
                                    <td>{{ data.invoice_no }}</td>
                                    <td>{{ data.invoice_date }}</td>
                                    <td>{{ data.supplier_name }}</td>
                                    <td>{{ data.customer_name }}</td>

                                    <td class="text-end">{{ data.amount }}</td>
                                    <td class="text-end">{{ data.total_amount }}</td>
                                    <td class="text-end">{{ data.amount_paid }}</td>
                                    <td class="text-end">{{ data.charge_fee }}</td>
                                    <td class="text-end">{{ data.amount_remaining }}</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                    <div class="row text-end">
                        <span style="font-weight: bold;color:green;">Total Profit: {{ others_totalPrfoit }}</span>
                    </div>
                    <div v-if="selectedCompany == 4">
                        Total Amount : {{ others_totalAmount }}<br />
                        Total Due: {{ others_total_due_amt }}<br />
                        Total Profit : {{ others_totalPrfoit }}<br />

                    </div>

                </div>
                <!-- END Consular -->
                <!-- row -->

                <div v-if="!selectedCompany">
                    Total Profit: {{ totalProfitSum.toFixed(2) }}<br />
                    Total Due: {{ formatAmount(totalDueAmountSumCalculated) }}<br />
                    Total Amount : {{ totalAmuntOfAllcompany.toFixed(2) }}<br />


                    <hr />
                    Due Brakedown<br />
                    <p class="text-primary">Travel Total Due Amt: {{ travel_total_due_amt }}</p>
                    <p class="text-success">Money Transfer Total Due Amt: {{ totalMoneyDueAmount }}</p>
                    <p class="text-danger">Consular Total Due Amt: {{ consular_total_due_amt }}</p>
                    <p class="text-warning">Others Total Due Amt: {{ others_total_due_amt }}</p>

                    <hr />
                    Total Amount Breakdown: <br />
                    Customer Amount (travel) : {{ travel_customer_amount }} + Total sale(money transfer): {{
                        totals.receiving_divided_by_rate.toFixed(2) }} + Customer amount (consular): {{ consular_customer_amt }} +
                    Total amount (others) : {{ others_totalAmount }} <br />
                </div>

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
            selectedCompany: '',
            firstCompanyName: '',
            secondCompanyName: '',
            thirdCompanyName: '',
            fourthCompanyName: '',
            suplier_id: '',
            customers: [],
            supplierList: [],
            frm_date: null,
            to_date: null,
            total_fees: 0,
            travel_customer_amount: 0,
            total_profit: 0,
            others_amt_paid: 0,
            totalTransaction: 0,
            moneytransferDueAmt: 0,
            total_due: 0,
            money_total_profit: 0,
            consular_total_due_amt: 0,
            consular_customer_amt: 0,
            consular_total_amt: 0,
            others_total_due_amt: 0,
            others_totalPrfoit: 0,
            otherstotalAmount: 0,
            others_totalAmount: 0,
            consular_total_profit: 0,
            money_net_supplier_amnt: 0,
            travel_total_due_amt: 0,
            travel_total_amount_of_Sale: 0,
            travel_total_net_amount: 0,
            travel_total_profit: 0,
            travel_total_due: 0,
            money_total_sales: 0,
            customer_id: '',
            report: [],
            money_report: [],
            consular_report: [],
            companyList: [],
        };
    },
    computed: {
        totalAmuntOfAllcompany() {
            return (
                (this.travel_customer_amount || 0) +
                (this.totals?.receiving_divided_by_rate || 0) +
                (this.consular_customer_amt || 0) +
                (this.otherstotalAmount || 0)
            );
        },
        totalProfitSum() {
            return (
                parseFloat(this.totalProfit || 0) +
                parseFloat(this.money_total_profit || 0) +
                parseFloat(this.consular_total_profit || 0) +
                parseFloat(this.others_totalPrfoit || 0)
            );
        },
        totalDueAmountSumCalculated() {
            return (
                parseFloat(this.travel_total_due_amt || 0) +
                parseFloat(this.totalMoneyDueAmount || 0) +
                parseFloat(this.consular_total_due_amt || 0) +
                parseFloat(this.others_total_due_amt || 0)
            );
        },
        totalProfit() {
            const sum = this.report.reduce((acc, item) => acc + Number(item.profit || 0), 0)
            return sum.toFixed(2)
        },
        totalMoneyDueAmount() {
            let totalInCents = 0;
            this.money_report.forEach(data => {
                const receivingAmount = parseFloat(data.receiving_amount) || 0;
                const rate = parseFloat(data.rate) || 1;
                const fees = parseFloat(data.fees) || 0;
                const othersFees = parseFloat(data.others_fees) || 0;
                const customerDeposit = parseFloat(data.customer_deposit) || 0;

                const sale = receivingAmount / rate;
                const total = sale + fees + othersFees;
                const due = total - customerDeposit;

                // Convert to cents to prevent floating point issues
                const dueInCents = Math.round((due > 0 ? due : 0) * 100);
                totalInCents += dueInCents;
            });

            // Convert back to decimal with two digits
            return (totalInCents / 100).toFixed(2);
        },
        totals() {
            return this.money_report.reduce((acc, data) => {
                const receivingAmount = parseFloat(data.receiving_amount) || 0;
                const rate = parseFloat(data.rate) || 1; // Avoid divide by zero
                const fees = parseFloat(data.fees) || 0;
                const othersFees = parseFloat(data.others_fees) || 0;
                const profit = parseFloat(data.profit) || 0;
                const customer_deposit = parseFloat(data.customer_deposit) || 0;

                const sale = receivingAmount / rate;
                const total = sale + fees + othersFees;
                acc.receiving_amount += receivingAmount;
                acc.rate += rate;
                acc.receiving_divided_by_rate += sale;
                acc.fees += fees;
                acc.others_fees += othersFees;
                acc.total_calculated += total;
                acc.profit += profit;
                acc.customer_deposit += customer_deposit;
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
        // Ensure values are formatted as yyyy-mm-dd
        this.getCompanyList();
        // this.getSuplierList();
        this.filterReport();

    },
    methods: {
        formatAmount(amount) {
            return Number(amount).toLocaleString('en-US', {
                minimumFractionDigits: 2,
                maximumFractionDigits: 2
            });
        },
        filterReport() {
            let company_id = $(".company_id").val();
            // First, hide all sections
            $(".for_travel, .for_moneytransfer, .for_consular, .for_others").hide();
            if (company_id == 1) {
                this.SearchTravelData();
                $(".for_travel").show();
            } else if (company_id == 2) {
                this.SearchDataMoney();
                $(".for_moneytransfer").show();
            } else if (company_id == 3) {
                this.SearchDataConsular();
                $(".for_consular").show();
            } else if (company_id == 4) {
                this.searchOthersData();
                $(".for_others").show();
            } else {
                // Show all if company_id is empty or unmatched
                this.SearchTravelData();
                this.SearchDataMoney();
                this.SearchDataConsular();
                this.searchOthersData();
                $(".for_travel, .for_moneytransfer, .for_consular, .for_others").show();
            }

        },

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
            //console.log("Data:" + data);
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
                // Access the first index
                if (this.companyList.length > 0) {
                    const cname = this.companyList[0].company_name;
                    this.firstCompanyName = cname;
                    //console.log("First Company Name:", cname);
                }

                // Access the second index
                if (this.companyList.length > 1) {
                    const cname = this.companyList[1].company_name;
                    this.secondCompanyName = cname;
                    // console.log("First Company Name:", cname);
                }
                // Access the third index
                if (this.companyList.length > 2) {
                    const cname = this.companyList[2].company_name;
                    this.thirdCompanyName = cname;
                    // console.log("First Company Name:", cname);
                }
                // Access the third index
                if (this.companyList.length > 3) {
                    const cname = this.companyList[3].company_name;
                    this.fourthCompanyName = cname;
                    // console.log("First Company Name:", cname);
                }

            });
        },

        customerlist() {
            axios.get('/api/category/allCustomers').then(response => {
                this.customers = response.data.data;
            });
        },

        fetchSuppliers() {
            if (!this.selectedCompany) {
                this.supplierList = [];
                return;
            }

            axios.get('/api/category/getSuplierConditionWise', {
                params: {
                    company_id: this.selectedCompany
                }
            })
                .then(response => {
                    this.supplierList = response.data.data;
                })
                .catch(error => {
                    console.error('Error fetching suppliers:', error);
                    this.supplierList = [];
                });
        },
        searchOthersData() {
            $(".loadingvideo").show();
            const formData = new FormData();
            let frm_date = $(".frm_date").val();
            let to_date = $(".to_date").val();
            let customer_id = $(".customer_id").val();
            let company_id = $(".company_id").val();
            let suppler_id = this.suplier_id;
            //console.log("Supplier: " + suppler_id);
            formData.append('suppler_id', suppler_id);
            formData.append('frm_date', frm_date);
            formData.append('to_date', to_date);
            formData.append('customer_id', customer_id);
            formData.append('company_id', company_id);

            const headers = {
                'Content-Type': 'multipart/form-data'
            };
            axios.post('/api/post/othersProfitReport',
                formData, {
                headers
            }).then((res) => {
                $(".loadingvideo").hide();

                this.others_report = res.data.data;
                this.others_amt_paid = res.data.total_sum;
                this.others_total_due_amt = res.data.total_due_amt.toFixed(2);
                this.others_totalPrfoit = res.data.totalPrfoit.toFixed(2);
                this.others_totalAmount = res.data.totalAmount.toFixed(2);
                this.otherstotalAmount = res.data.totalAmount;
                // console.log(res.data.total_sum);
                this.frm_date = frm_date;
                this.to_date = to_date;
                this.company_id = company_id;
            }).catch(e => {
                this.notifmsg = e.response.data
            });
        },


        SearchTravelData() {
            $(".loadingvideo").show();
            const formData = new FormData();
            let frm_date = $(".frm_date").val();
            let to_date = $(".to_date").val();
            let customer_id = $(".customer_id").val();
            let company_id = $(".company_id").val();
            let suppler_id = this.suplier_id;
            //console.log("Supplier: " + suppler_id);
            formData.append('suppler_id', suppler_id);
            formData.append('frm_date', frm_date);
            formData.append('to_date', to_date);
            formData.append('customer_id', customer_id);
            formData.append('company_id', company_id);

            const headers = {
                'Content-Type': 'multipart/form-data'
            };
            axios.post('/api/post/travelProfitReport',
                formData, {
                headers
            }).then((res) => {
                $(".loadingvideo").hide();

                this.report = res.data.data;
                this.total_profit = res.data.total_sum;
                this.travel_total_due_amt = res.data.total_due_amt;
                this.travel_customer_amount = res.data.customer_amount
                this.travel_total_amount_of_Sale = res.data.total_amount_of_Sale
                this.travel_total_net_amount = res.data.total_net_amount
                this.travel_total_profit = res.data.total_profit
                this.travel_total_due = res.data.total_due

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
            let frm_date = $(".frm_date").val();
            let to_date = $(".to_date").val();
            let customer_id = $(".customer_id").val();
            let company_id = $(".company_id").val();
            let suppler_id = this.suplier_id;
            //console.log("Supplier: " + suppler_id);
            formData.append('suppler_id', suppler_id);
            formData.append('frm_date', frm_date);
            formData.append('to_date', to_date);
            formData.append('customer_id', customer_id);
            formData.append('company_id', company_id);
            const headers = {
                'Content-Type': 'multipart/form-data'
            };
            axios.post('/api/post/moneyProfitReport',
                formData, {
                headers
            }).then((res) => {
                $(".loadingvideo").hide();
                console.log("Net Supplier Amount: " + res.data.net_supplier_amnt);
                this.money_report = res.data.data;
                this.money_total_profit = res.data.total_sum;
                this.moneytransferDueAmt = res.data.moneytransferDueAmt.toFixed(2);
                this.money_net_supplier_amnt = res.data.net_supplier_amnt;
                this.total_fees = res.data.total_fees;
                this.money_total_sales = res.data.totalsales
                // console.log(res.data.total_sum);
                this.total_due = res.data.total_due;
                this.totalTransaction = res.data.totalTransaction;
                this.frm_date = frm_date;
                this.to_date = to_date;
                this.company_id = company_id;
            }).catch(e => {
                if (e.response && e.response.data) {
                    this.notifmsg = e.response.data;
                } else if (e.request) {
                    this.notifmsg = 'No response from server. Please check your connection or try again later.';
                } else {
                    this.notifmsg = 'An unexpected error occurred: ' + e.message;
                }
            });
        },
        SearchDataConsular() {
            $(".loadingvideo").show();
            const formData = new FormData();
            let frm_date = $(".frm_date").val();
            let to_date = $(".to_date").val();
            let customer_id = $(".customer_id").val();
            let company_id = $(".company_id").val();
            let suppler_id = this.suplier_id;
            //console.log("Supplier: " + suppler_id);
            formData.append('suppler_id', suppler_id);
            formData.append('frm_date', frm_date);
            formData.append('to_date', to_date);
            formData.append('customer_id', customer_id);
            formData.append('company_id', company_id);
            const headers = {
                'Content-Type': 'multipart/form-data'
            };
            axios.post('/api/post/profitReportConsular',
                formData, {
                headers
            }).then((res) => {
                $(".loadingvideo").hide();
                //    console.log(res.data.data);
                this.consular_report = res.data.data;
                this.consular_total_profit = res.data.total_sum.toFixed(2);
                this.consular_total_due_amt = res.data.total_due_amt.toFixed(2);
                this.consular_total_amt = res.data.totalAmount.toFixed(2);
                this.consular_customer_amt = res.data.totalAmount;
                // console.log(res.data.total_sum);
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
