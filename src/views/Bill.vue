<template>
    <AppHeader />
    <div class="row  d-flex justify-content-center mt-2">
        <div class="col-md-10">
            <div class="input-group">
                <input type="text" class="form-control text-violet" placeholder="Nhập thông tin cần tìm"
                    v-model="searchText" />
                <div class="input-group-append">
                    <button class="btn btn-outline-violet text-white" type="button">
                        <i class="fas fa-search"></i> Tìm kiếm
                    </button>
                </div>
            </div>
        </div>
    </div>
    <div class="mt-2 row d-flex justify-content-around">
        <div class="col-2 backgound-violet rounded">
            <div class="card ">
                <ul class="list-group list-group-flush justify-content-center m-2">
                    <li class="list-group-item"><a href="" class="text-center text-violet">Nhân viên </a></li>
                    <li class="list-group-item">
                        <router-link :to="{
                            name: 'home',
                        }">
                            <a href="" class="text-center text-violet">Sản phẩm</a>
                        </router-link>
                    </li>
                    <li class="list-group-item">
                        <router-link :to="{
                            name: 'bill',
                        }">
                            <a href="" class="text-center text-violet">Đơn hàng</a>
                        </router-link>
                        </li>
                    <li class="list-group-item"><a href="" class="text-center text-violet">Doanh thu </a></li>
                    <li class="list-group-item">
                        <router-link :to="{
                                name: 'cart',
                            }">
                            <a href="" class="text-center text-violet">Sản phẩm đã được chọn</a>
                        </router-link>
                    </li>
                </ul>
            </div>
        </div>
        <div class="col-9">
            <div class="mr-auto text-violet ">
                <h4><i class="far fa-chart-bar"></i> Hoạt động hôm nay:</h4>
            </div>
            <div class="row">
                <div class=" card backgound-violet col-3 ml-2">
                    <div class="card-body">
                        <h6 class="text-white"><i class="fas fa-shopping-cart text-white"></i> Số đơn hàng: 0</h6>
                    </div>
                </div>
                <div class=" card backgound-violet col-3 ml-2">
                    <div class="card-body">
                        <h6 class="text-white"><i class="fas fa-redo text-white"></i> Khách trả hàng: 0</h6>
                    </div>
                </div>
            </div>
            <div class="row">
                <div class="col-8">
                    <table class="table mt-2">
                        <thead class="backgound-violet text-white">
                            <tr>
                                <th scope="col">Ngày thuê</th>
                                <th scope="col">Ngày trả</th>
                                <th scope="col">Mã HD</th>
                                <th scope="col">Tên KH</th>
                                <th scope="col">SDT</th>
                                <th scope="col">Tình trạng</th>
                            </tr>
                        </thead>
                        <tbody  v-for="(bill, index) in ketqualoc" :key="bill._id">
                            <tr>
                                <td>{{ bill.ngaymuon }}</td>
                                <td>{{ bill.ngaytra }}</td>
                                <td>{{ bill._id }}</td>
                                <td>{{ bill.nameCustomer }}</td>
                                <td>{{ bill.phone }}</td>
                                <td v-if="bill.tinhTrang">đã trả</td>
                                <td v-else> đang thuê </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
    <AppFooter />
</template>
<script>
import AppHeader from "../components/AppHeader.vue";
import AppFooter from "../components/AppFooter.vue";
import axios from "axios";
import { reactive, computed, ref } from "vue";
import { useRouter } from "vue-router";

export default {
    components: {
        AppHeader,
        AppFooter,
    },
    setup() {
        const router = useRouter();
        const data = reactive({
            listBill: [],
            bill: {}
        });
        const searchText = ref("");
        console.log(Object.keys(data.bill).length);
        //ref => data= ref(2) =>data.value = 3
        //data = reactive([]); => data.push(1,2);
        async function getAllBills() {
            const response = await axios.get("http://localhost:3000/api/bill");
            data.listBill = response.data;
        }
        getAllBills();
    //     async function chooseItem(id) {
    //         const response = await axios.get(`http://localhost:3000/api/item/${id}`);
    //         data.item = response.data;
    //     }
        let ketqualoc = computed(() => {
            return data.listBill.filter((e) => e.nameCustomer.toUpperCase().includes(searchText.value.toUpperCase()) || e.phone.toUpperCase().includes(searchText.value.toUpperCase()));
        })
        return {
            ketqualoc,
            searchText,
            data,
        }
    }
};
</script>
