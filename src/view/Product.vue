<template>
  <div>
    <table class="table">
      <thead>
        <tr class="text-center">
          <th scope="col">商品名稱</th>
          <th scope="col">商品圖片</th>
          <th scope="col">商品類型</th>
          <th scope="col">價格</th>
          <th scope="col">庫存</th>
          <th scope="col">操作</th>
        </tr>
      </thead>
      <tbody>
        <tr class="text-center" v-for="(item, key) in productList" :key="key">
          <th scope="row">{{ item.title }}</th>
          <td>
            <img :src="imgUrl" class="imgclass" />
          </td>
          <td>{{ item.type }}</td>
          <td>{{ item.price }}</td>
          <td>{{ item.amount }}</td>
          <td>
            <button
              type="button"
              class="btn editBtn"
              data-bs-toggle="modal"
              data-bs-target="#exampleModal"
            >
              編輯
            </button>
            <button
              type="button"
              class="btn deletBtn m-lg-2"
              @click="removeProduct(key)"
            >
              刪除
            </button>
          </td>
        </tr>
        <!-----------------新增----------------->
        <tr>
          <td scope="row" colspan="8" class="text-center">
            <button
              data-bs-toggle="modal"
              data-bs-target="#plus"
              class="plus-set"
            >
              <i class="fas fa-plus fa-1x"></i>
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <footer class="footer">
      <pagination />
    </footer>
  </div>
  <!----------------------------- 編輯Modal ---------------------------------->
  <div
    class="modal fade"
    id="exampleModal"
    tabindex="-1"
    aria-labelledby="exampleModalLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog ml-550 mt-130">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title fw-bold" id="exampleModalLabel">編輯商品</h5>
          <button
            type="button"
            class="btn btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body">
          <table class="table">
            <thead>
              <tr class="text-center">
                <th scope="col">商品名稱</th>
                <th scope="col">商品圖片</th>
                <th scope="col">商品類型</th>
                <th scope="col">價格</th>
                <th scope="col">庫存</th>
              </tr>
            </thead>
            <tbody>
              <tr class="text-center" v-for="item in productList">
                <td scope="row">
                  <input type="text" v-model="item.title" />
                </td>
                <td>
                  <div class="d-flex justify-content-center">
                    <div class="mr-10 btn-close" aria-label="Close"></div>
                    <img
                      src="../assets/img/機械暴龍獸.jpg"
                      alt=""
                      class="w-75"
                    />
                  </div>
                </td>

                <td>
                  <input type="text" v-model="item.type" />
                </td>
                <td>
                  <input type="text" v-model="item.price" />
                </td>
                <td>
                  <input type="text" v-model="item.amount" />
                </td>
              </tr>
            </tbody>
          </table>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn beclose" data-bs-dismiss="modal">
            關閉
          </button>
          <button type="button" class="btn save-btn">儲存</button>
        </div>
      </div>
    </div>
  </div>
  <!----------------------------- 新增Modal ---------------------------------->
  <div
    class="modal fade"
    id="plus"
    tabindex="-1"
    aria-labelledby="exampleModalLabel"
    aria-hidden="true"
  >
    <div class="modal-dialog ml-550 mt-130">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title fw-bold">新增商品</h5>
          <button
            type="button"
            class="btn-close"
            data-bs-dismiss="modal"
            aria-label="Close"
          ></button>
        </div>
        <div class="modal-body">
          <table class="table">
            <thead>
              <tr class="text-center">
                <th scope="col" class="product">商品名稱</th>
                <th scope="col">商品圖片</th>
                <th scope="col">商品類型</th>
                <th scope="col">價格</th>
                <th scope="col">庫存</th>
              </tr>
            </thead>
            <tbody>
              <tr class="text-center">
                <td scope="row">
                  <input
                    type="text"
                    placeholder="請輸入名稱"
                    v-model="newName"
                  />
                </td>
                <td class="d-flex">
                  <div class="d-flex flex-column">
                    <input type="file" @change="fileSelected" />
                    <img v-if="image" :src="image" width="100" height="100" />
                    <button @click="upload">上傳</button>
                  </div>
                </td>
                <td>
                  <input
                    type="text"
                    placeholder="請輸入類型"
                    v-model="newType"
                  />
                </td>
                <td>
                  <input
                    type="text"
                    placeholder="請輸入價格"
                    v-model="newPrice"
                  />
                </td>
                <td>
                  <input
                    type="text"
                    placeholder="請輸入庫存"
                    v-model="newAmount"
                  />
                </td>
              </tr>
            </tbody>
          </table>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn beclose" data-bs-dismiss="modal">
            關閉
          </button>
          <button
            type="button"
            class="btn save-btn"
            @click="addProduct"
            @keyup.enter="addProduct"
          >
            新增
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import pagination from "@/components/pagination.vue";

export default {
  components: {
    pagination,
  },

  data() {
    return {
      imgUrl: new URL("../assets/img/機械暴龍獸.jpg", import.meta.url),
      newName: "",
      newType: "",
      newPrice: "",
      newAmount: "",
      productList: [
        {
          title: "機械暴龍獸",
          type: "模型",
          price: "$1950",
          amount: "10",
        },
      ],
    };
  },

  methods: {
    addProduct() {
      let nameValue = this.newName.trim();
      let typeValue = this.newType.trim();
      let priceValue = this.newPrice.trim();
      let amountValue = this.newAmount.trim();
      if (!nameValue || !typeValue || !priceValue || !amountValue) {
        return;
      }
      this.productList.push({
        title: nameValue,
        type: typeValue,
        price: priceValue,
        amount: amountValue,
      });
      this.newName = "";
      this.newType = "";
      this.newPrice = "";
      this.newAmount = "";
    },
    removeProduct(key) {
      this.productList.splice(key, 1);
    },
    removeImg() {
      this;
    },
    fileSelected(e) {
      const file = e.target.files.item(0);
      const reader = new FileReader();
      reader.addEventListener("load", this.imageLoaded);
      reader.readAsDataURL(file);
    },
    imageLoaded(e) {
      this.image = e.target.result;
    },
    upload() {
      axios.post("/upload", { image: this.image });
    },
  },
};
</script>

<style lang="scss" scoped>
.table {
  margin-top: 40px;
  background: #fff;
  border-radius: 10px;
  .editBtn {
    @include orange-button;
  }
  .deletBtn {
    @include delete-button;
  }
  .imgclass {
    width: 50px;
    height: 50px;
  }
  .plus-set {
    border-radius: 20%;
    border: 0px solid #fff;
    &:hover {
      background-color: var(--button-color1);
      color: #fff;
    }
  }
}

.modal-content {
  width: 950px;
  tbody {
    button {
      width: 80px;
      height: 30px;
      border: 1px solid gray;
      &:hover {
        background-color: #3e3e3e;
        color: #fff;
      }
    }
  }
  .btn-close {
    border: 0px solid #fff;
    cursor: pointer;
  }
  .beclose {
    @include delete-button;
  }
  .save-btn {
    @include orange-button;
  }
  button i {
    border: 0px #fff solid;
  }
  tr {
    td {
      &:nth-child(1) input {
        width: 240px;
        text-align: center;
      }
      &:nth-child(2) input,
      &:nth-child(3) input,
      &:nth-child(4) input,
      &:nth-child(5) input {
        width: 110px;
        text-align: center;
      }
    }
  }
}
</style>
