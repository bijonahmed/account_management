<template>
    <div class="main-wrapper">
        <div class="page-wrapper">
            <Nav />

            <div id="printableArea">

                <div class="page-content" style="background-color: white;">
                    <h3><center>Money Transfer Invoice</center></h3>
                    <br /><br />
                    <h3>Transaction Ref:{{ customerInfo.transaction_ref }}</h3>
                    <!-- Start -->
                    <table width="100%" border="0" align="center">
                        <tr>
                            <td width="33%">
                                <div align="center"><img :src="`/456.jpg`" width="300" height="250" /> </div>
                            </td>
                            <td width="31%">&nbsp;</td>
                            <td width="36%">{{ customerInfo.customername }}<br />
                              {{ customerInfo.customeraddres }}<br />
                                {{ customerInfo.customerphone }} </td>
                        </tr>

                        <tr>
                            <td height="40" colspan="3" valign="top"><span class="style3"><strong  style="font-size: 25px;">{{ cdata.company_name }}</strong>  <br/>
                                <strong>{{ cdata.address }}  <br/>{{ cdata.tel }}<br/>Email: {{ cdata.email }} <br/></strong></span> <span
                                    class="style6"></span> <span class="style3"><br />
                                Invoice Ref</span><span class="d-none style3">:{{
                                    customerInfo.mone_transfer_id }}</span><span class="style3">Invoice Date:{{ customerInfo.date }}</span>                            </td>
                        </tr>
</table>

                    <table width="100%" border="0">
                        <tr>
                            <td height="60" valign="top"><span class="style9"><span class="style2"><span class="style3"><span class="style4"><span class="style5"><span class="style7"><strong>Beneficiary name:</strong>{{
                                    customerInfo.beneficiary_name }}<strong><br />
                                    Beneficiary City:</strong>{{ customerInfo.beneficiary_city }}<strong><br />
                                        Beneficiary Country</strong>:{{ customerInfo.beneficiary_country
                                    }}<strong><br />
                                        Beneficiary Contact Number</strong>:{{ customerInfo.beneficary_contact_num
                                    }}<br />
                                    <strong>Payment Receive:</strong>{{ customerInfo.payment_receive }}<strong><br />
                                        Payment Type:
                                    </strong>Bkash<strong><br />
                                        Number: </strong>{{ customerInfo.bkash_number }}
                                    </span></span></span></span></span>
                                    <span class="style7" v-if="customerInfo.payment_type == 2"><br />
                                        <strong>Bank Transfer, Bank : </strong>{{
                                            customerInfo.bank_name }}<br />
                                        <strong> Branch : </strong>{{ customerInfo.branch_name }},<strong><br />
                                        Account No : </strong>{{ customerInfo.acc_no }}<br>
                                    </span>
                                    <span class="style7" v-if="customerInfo.payment_type == 3"> <strong>Cash
                                                Pickup Bank Name :</strong> {{ customerInfo.bank_name }},<strong> Pickup
                                                Bank Branch : </strong>{{ customerInfo.branch_name }}</span><span v-if="customerInfo.payment_type == 3"><span class="style9"><br>

                            </span></span>
                          </span> </td>
                            <td width="36%" colspan="2" rowspan="2" valign="top"><span class="style9 style7">Rate:{{ customerInfo.rate }}<br />
                                Fees:{{ customerInfo.fees
                                    }}</span><span class="style7"><br />
                                Others Fees:{{ customerInfo.others_fees }}<br />
                                Receiving BD Amount</span><span class="style9 style7">:{{
                                    customerInfo.receiving_amount }}<br />
                                Customer Amount:0.00<br />
                                    Customer Amount:{{ customerInfo.total_amount }}<br />
                                Customer Deposit:{{
                                    customerInfo.customer_deposit }}</span>                          </td>
                        </tr>


                        <tr>
                            <td><span class="style9 style7">Net Amount:{{
                                    customerInfo.net_amount }}Profit:{{ customerInfo.profit }}Due
                                    Amount:{{ customerInfo.due_amount }}</span></td>
                        </tr>
                    </table>

                    <table width="100%" border="0">

                        <tr>
                          <td>&nbsp;</td>
                          <td>&nbsp;</td>
                          <td>&nbsp;</td>
                        </tr>
                        <tr>
                          <td>&nbsp;</td>
                          <td>&nbsp;</td>
                          <td>&nbsp;</td>
                        </tr>
                        <tr>
                            <td width="12%"><strong>_________________________</strong></td>
                            <td width="75%">&nbsp;</td>
                            <td width="13%"><strong>_________________________</strong></td>
                        </tr>
                        <tr>
                            <td>
                                <div align="center">Signature</div>                            </td>
                            <td>&nbsp;</td>
                            <td>
                                <div align="center">Signature</div>                            </td>
                        </tr>
                        <tr>
                            <td>&nbsp;</td>
                            <td>
                                <div align="center"><strong>Invoice By: {{ customerInfo.entry_by }}</strong></div>                            </td>
                            <td>&nbsp;</td>
                        </tr>
                    </table>
                    <br/>
                    <p>_______________________________________________________________________________________________________________________________________________________________________________________________________________
                    </p>
                    <br/>
                    <h3>Transaction Ref:{{ customerInfo.transaction_ref }}</h3>
                    <table width="100%" border="0" align="center">
                        <tr>
                            <td width="33%">
                                <div align="center"><img :src="`/456.jpg`" width="300" height="250" /> </div>
                            </td>
                            <td width="34%">&nbsp;</td>
                            <td width="33%"><strong>{{ customerInfo.customername }}</strong><br />
                                <span class="style1">{{ customerInfo.customeraddres }}<br />
                                    {{ customerInfo.customerphone }}</span>
                          </td>
                        </tr>

                        <tr>
                            <td height="40" colspan="3" valign="top"><span class="style3"><strong  style="font-size: 25px;">{{ cdata.company_name }}</strong>  <br/>
                                <strong>{{ cdata.address }}  <br/>{{ cdata.tel }}<br/>Email: {{ cdata.email }} <br/></strong></span> <span
                                    class="style6"></span> <span class="style3"><br />
                                Invoice Ref</span><span class="d-none style3">:{{
                                    customerInfo.mone_transfer_id }}</span><span class="style3">Invoice Date:{{ customerInfo.date }}</span>                            </td>
                        </tr>
                    </table>

                    <table width="100%" border="0">
                        <tr>
                            <td colspan="4" valign="top"><span class="style9"><span class="style5"><span class="style7"><strong>Beneficiary name:</strong>{{
                                    customerInfo.beneficiary_name }} <br />
                                    <strong>Beneficiary City:</strong>{{ customerInfo.beneficiary_city }}<br />
                                    <strong>Beneficiary Country</strong>:{{ customerInfo.beneficiary_country }},
                                    <strong><br />
                                        Beneficiary Contact Number</strong>:{{ customerInfo.beneficary_contact_num
                                    }}<br />
                                    <strong>Payment Receive:</strong>{{ customerInfo.payment_receive }}<br />
                                    <strong>Payment Type:
                                        Bkash, Number: </strong>{{ customerInfo.bkash_number }}
                                    </span></span>
                                    <span class="style7" v-if="customerInfo.payment_type == 2"><strong><br />
                                                Bank Transfer, Bank : </strong>{{ customerInfo.bank_name }}<strong>
                                                <br />
                                                Branch : </strong>{{ customerInfo.branch_name }}<strong><br />
                                                Account No : </strong>{{ customerInfo.acc_no }}<br>
                                    </span>
                                    <span class="style7" v-if="customerInfo.payment_type == 3"> <strong>Cash
                                                Pickup Bank Name :</strong> {{ customerInfo.bank_name }},<strong> Pickup
                                                Bank Branch : </strong>{{ customerInfo.branch_name }}</span><span v-if="customerInfo.payment_type == 3"><span class="style9"><br>

                                        </span></span>
                          </span> </td>
                            <td width="33%" colspan="2" rowspan="2" valign="top"><span class="style9 style7">Rate:{{ customerInfo.rate }}<br />
                            Fees:{{ customerInfo.fees
                                    }}</span><span class="style7"><br />
                                Others Fees:{{ customerInfo.others_fees }}<br />
                                Receiving BD Amount</span><span class="style9 style7">:{{
                                    customerInfo.receiving_amount }}<br />
                                Customer Amount:0.00<br />
                                Customer Amount:{{
                                    customerInfo.total_amount }}<br />
                                Customer Deposit:{{
                                    customerInfo.customer_deposit }}</span>                            </td>
                        </tr>


                        <tr>
                            <td colspan="4" rowspan="2"><span class="style9 style7">Net Amount:{{
                                    customerInfo.net_amount }}Profit:{{ customerInfo.profit }}Due
                                    Amount:{{ customerInfo.due_amount }}</span></td>
                        </tr>
                        <tr style="display: none;">
                            <td colspan="2">&nbsp;</td>
                        </tr>
                    </table>

                    <table width="100%" border="0">

                        <tr>
                          <td>&nbsp;</td>
                          <td>&nbsp;</td>
                          <td>&nbsp;</td>
                        </tr>
                        <tr>
                          <td>&nbsp;</td>
                          <td>&nbsp;</td>
                          <td>&nbsp;</td>
                        </tr>
                        <tr>
                            <td width="12%"><strong>_________________________</strong></td>
                            <td width="75%">&nbsp;</td>
                            <td width="13%"><strong>_________________________</strong></td>
                        </tr>
                        <tr>
                            <td>
                                <div align="center">Signature</div>                            </td>
                            <td>&nbsp;</td>
                            <td>
                                <div align="center">Signature</div>                            </td>
                        </tr>
                        <tr>
                            <td>&nbsp;</td>
                            <td>
                                <div align="center"><strong>Invoice By: {{ customerInfo.entry_by }}</strong></div>                            </td>
                            <td>&nbsp;</td>
                        </tr>
                    </table>
                    <p>&nbsp;</p>
                    <p align="right">&nbsp;</p>
                </div>

                <!-- END -->






            </div>
            <div class="container">
                <div class="row">
                    <div class="col-4">
                        <button type="button" class="btn btn-primary me-2 w-100 submit_btn"
                            @click="printDiv('printableArea')">Print</button>
                    </div>
                    <div class="col-4">
                        <button type="button" class="btn btn-danger me-2 w-100 submit_btn"
                            @click="reloading">Reload</button>
                    </div>
                    <div class="col-4">
                        <button type="button" class="btn btn-success me-2 w-100" @click="newInvoice">New
                            Invoice</button>
                    </div>
                </div>
                <br /><br />
            </div>

        </div>
    </div>


</template>

<script>
export default {
    components: {},
    data() {
        return {
            cdata: {
                company_name: '',
                address: '',
                email: '',
                tel: '',
            },
            notifmsg: '',
            customerInfo: {
                payment_receive: '',
                transaction_ref: '',
                entry_by: '',
                beneficiary_name: '',
                beneficiary_city: '',
                beneficiary_country: '',
                beneficary_contact_num: '',
                payment_type: '',
                bkash_number: '',
                bank_name: '',
                branch_name: '',
                acc_no: '',
                others_fees: '',
                rate: '',
                fees: '',
                receiving_amount: '',
                total_amount: '',
                customername: '',
                customeraddres: '',
                customerphone: '',
            },
            itemlist: [],
            cartItem: [],
        }
    },
    created() {
        this.findInvoiceId();
        this.findRowId();
    },
    methods: {
        findInvoiceId() {
            this.axios
                .get(`/api/category/getInvoiceMoneyData/${this.$route.params.id}`)
                .then((response) => {

                    console.log("ddd" + response.data.transaction_ref);
                    //Invoice
                    this.customerInfo.mone_transfer_id = response.data.data.mone_transfer_id;
                    this.customerInfo.date = response.data.invoice_date;
                    this.customerInfo.entry_by = response.data.entry_by;
                    this.customerInfo.transaction_ref = response.data.transaction_ref;
                    //customer 
                    this.customerInfo.customername = response.data.data.name;
                    this.customerInfo.customeraddres = response.data.data.addres;
                    this.customerInfo.customerphone = response.data.data.phone;
                    //end
                    this.customerInfo.payment_receive = response.data.data.payment_receive;
                    this.customerInfo.beneficiary_name = response.data.data.beneficiary_name;
                    this.customerInfo.beneficiary_city = response.data.data.beneficiary_city;
                    this.customerInfo.beneficiary_country = response.data.data.beneficiary_country;
                    this.customerInfo.beneficary_contact_num = response.data.data.beneficary_contact_num;
                    this.customerInfo.payment_type = response.data.data.payment_type;
                    this.customerInfo.bkash_number = response.data.data.bkash_number;
                    this.customerInfo.bank_name = response.data.data.bank_name;
                    this.customerInfo.branch_name = response.data.data.branch_name;
                    this.customerInfo.acc_no = response.data.data.acc_no;
                    this.customerInfo.rate = response.data.data.rate;
                    this.customerInfo.fees = response.data.data.fees;
                    this.customerInfo.receiving_amount = response.data.data.receiving_amount;
                    this.customerInfo.total_amount = response.data.data.total_amount;
                    this.customerInfo.others_fees = response.data.data.others_fees;
                    this.customerInfo.net_amount = response.data.data.net_amount;
                    this.customerInfo.profit = response.data.data.profit;
                    this.customerInfo.customer_deposit = response.data.data.customer_deposit;
                    this.customerInfo.due_amount = response.data.data.due_amount;

                });
        },
        findRowId() {
            this.axios
                .get(`/api/user/getCompanySetting`)
                .then((response) => {
                    this.cdata.company_name = response.data.data.company_name;
                    this.cdata.address = response.data.data.address;
                    this.cdata.email = response.data.data.email;
                    this.cdata.tel = response.data.data.tel;
                });
        },
        printDiv(divName) {
            var printContents = document.getElementById(divName).innerHTML;
            var originalContents = document.body.innerHTML;
            document.body.innerHTML = printContents;
            window.print();
            document.body.innerHTML = originalContents;
        },
        newInvoice() {
            this.$router.push('/invoice/addnewinvoiceMoney');
        },
        reloading() {
            location.reload()
        }
    }
}
</script>

<style>
.style1 {
    font-weight: bold
}

.style2 {
    font-size: 14px
}

.style3 {
    font-size: 18px
}

.style4 {
    font-size: 18
}

.style5 {
    font-size: 16px
}

.style6 {
    font-weight: bold;
    font-size: 18px;
}

.style7 {
    font-size: 17px
}

.style1 {
    font-family: Verdana, Arial, Helvetica, sans-serif;
    font-size: 13px;
}

.style4 {
    font-family: Verdana, Arial, Helvetica, sans-serif;
    font-size: 11px;
}

.style6 {
    font-size: 12px;
    font-family: Verdana, Arial, Helvetica, sans-serif;
}

.style9 {
    /* font-family: Verdana, Arial, Helvetica, sans-serif;
    font-size: 12px; */
}
</style>
