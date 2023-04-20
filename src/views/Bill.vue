<template>
    <div class="row  d-flex justify-content-center">
        <div class="col-md-10">
            <InputSearch v-model="searchText" />
        </div> 
    </div>
    <div class="mt-2 row d-flex justify-content-around">
        <div class="col-2 backgound-violet rounded">
            <div class="card " >
                <ul class="list-group list-group-flush justify-content-center m-2">
                    <li class="list-group-item"><a href="" class="text-center text-violet">Nhân viên </a></li>
                    <li class="list-group-item"><a href="" class="text-center text-violet">Sản phẩm </a></li>
                    <li class="list-group-item"><a href="./Bill.vue" class="text-center text-violet">Đơn hàng </a></li>
                    <li class="list-group-item"><a href="" class="text-center text-violet">Doanh thu </a></li>
                </ul>
            </div>
        </div>
        <div class="col-9">
            <div>
                <button class="btn btn-sm btn-violet mt-2" @click="goToAddBill">
                    <i class="fas fa-plus"></i> Thêm Bill
                </button>
            </div>
            <div class="row">
                <div class="col-7">
                    <BillList 
                    v-if="filteredBillsCount > 0" 
                    :bills="filteredBills" 
                    v-model:activeIndex="activeIndex" 
                    />
                    <!-- <p v-else>Không có liên hệ nào.</p>     -->
                    <p v-else>{{ filteredBills }}</p> 
                </div>
            </div>
        </div>
    </div>

</template>
<script>
import InputSearch from "@/components/InputSearch.vue";
import BillList from "@/components/BillList.vue";
import BillService from "@/services/bill.service";

export default {
    components: {
        InputSearch,
        BillList,
    },
    data() {
        return{
            bills: [],
            activeIndex: -1,
            searchText: "",
        };
    },
    watch: {
        searchText() {
            this.activeIndex = -1;
        },
    },
    computed: {
        //Chuyển đổi các đối tượng thành chuỗi để tiện tìm kiếm
        billStrings() {
            return this.bills.map((bill) => {
                const {nameCustomer, address, phone, indentification, ngaylap, ngaymuon, ngaytra, tinhTrang } = bill;
                return [nameCustomer, address, phone, indentification, ngaylap, ngaymuon, ngaytra, tinhTrang ].join("");
            });
        },
        //Trả về các Bill có chứa thông tin cần tìm
        filteredBills() {
            if (!this.searchText) return this.bills;
            return this.bills.filter((_bill, index) => 
                this.billStrings[index].includes(this.searchText)
            );
        },
        activeBill() {
            if (this.activeIndex <0) return null;
            return this.filteredBills[this.activeIndex];
        },
        filteredBillsCount() {
            return this.filteredBills.length;
        },
    },
    methods: {
        async retrieveBills() {
            try {
                this.bills = await BillService.getAll();
            } catch (error) {
                console.log(error);
            }
        },
        refreshList() {
            this.retrieveBills();
            this.activeIndex =-1;
        },
        goToAddBill() {
            this.$router.push({name: "bill.add"});
        },
    },
    mounted() {
        this.refreshList();
    },
};
</script>
