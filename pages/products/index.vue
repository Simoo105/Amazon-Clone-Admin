<template>
    <main>
        <div class= "container- fluid">
            <div class="row">
                <div class="col-sm-3"></div>
                <div class="col-sm-6">
                    <div class="a-section">
                        <div class="a-spacing-top-medium"></div>
                        <h2 style="text-align: center">Add new Product</h2>
                        <form  enctype="multipart/form-data">
                            <!-- Category Dropdown -->
                            <div class="a-spacing-top-medium">
                                <label >Category</label>
                                <select class="a-select-option" v-model="categoryID" name="" id="">
                                    <option v-for="category in Allcategories" :key="category._id" :value="category._id">{{category.type}}</option>
                                </select>
                            </div>
                            <!-- Owner Dropdown -->
                            <div class="a-spacing-top-medium">
                                <label >Owner</label>
                                <select class="a-select-option" v-model="ownerID" name="" id="">
                                    <option v-for="owner in Allowners" :key="owner._id" :value="owner._id">{{owner.name}}</option>
                                </select>
                            </div>
                            <!-- Title Input -->
                            <div class="a-spacing-top-medium">
                                <label style="margin-bottom: 0px;" >Title</label>
                                <input type="text" class="a-input-text" style ="width: 100%" v-model="title">
                            </div>
                            <!-- Price Input -->
                            <div class="a-spacing-top-medium">
                                <label style="margin-bottom: 0px;" >Price</label>
                                <input type="number" class="a-input-text" style ="width: 100%" v-model="price">
                            </div>
                            <!-- stockQuantity Input -->
                            <div class="a-spacing-top-medium">
                                <label style="margin-bottom: 0px;" >Stock Quantity</label>
                                <input type="number" class="a-input-text" style ="width: 100%" v-model="stockQuantity">
                            </div>
                            <!-- Description Input -->
                            <div class="a-spacing-top-medium">
                                <label style="margin-bottom: 0px;" >Description</label>
                                <textarea placeholder="Provide details such as a product description" name="" style="width: 100%" v-model="description"></textarea>
                            </div>
                            <!-- Photo Input -->
                            <div class="a-spacing-top-medium">
                                <label style="margin-bottom: 0px;" >Add Photo</label>
                                <div class="a-row a-spacing-top-medium">
                                    <label class="choosefile-button">
                                        <i class="fal fa-plus"></i>
                                        <input type="file" @change="onFileSelected">
                                        <p style="margin-top: -70px">{{fileName.substring(0,10)+ '...'}}</p>
                                    </label>
                                </div>
                            </div>
                            <hr>
                            <!-- Button -->
                            <div class="a-spacing-top-large">
                                <span class="a-button-register">
                                    <span class="a-button-inner">
                                        <span class="a-button-text" @click="onAddProduct">Add product</span>
                                    </span>
                                </span>
                            </div>
                        </form>
                    </div>
                </div>
                <div class="col-sm-3"></div>
            </div>
        </div>
    </main>
</template>
<script>
    export default {
        data() {
            return {
                categoryID: null,
                ownerID: null,
                title: '',
                price: 0,
                stockQuantity: 1,
                description: '',
                selectedFile:null,
                fileName: ''
            }
        },
        async asyncData({$axios}) {
            try {
                let categories = $axios.$get('http://localhost:3000/api/categories');
                let owners = $axios.$get('http://localhost:3000/api/owners');

                const [catResponse, ownerResponse] = await Promise.all([categories, owners]);
                return {
                    Allcategories: catResponse.categories,
                    Allowners: ownerResponse.owners
                }
            }
            catch (error) {
                console.log(error)
            }
        },
        methods: {
            onFileSelected(event) {
                this.selectedFile = event.target.files[0];
                console.log(this.selectedFile);
                this.fileName = event.target.files[0].name;
            },
            async onAddProduct() {
                let data = new FormData();
                data.append("title", this.title);
                data.append("price", this.price);
                data.append("description", this.description);
                data.append("ownerID", this.ownerID);
                data.append("stockQuantity", this.stockQuantity);
                data.append("categoryID", this.categoryID);
                data.append("photo", this.selectedFile, this.selectedFile.name);

                try {
                    let result = await this.$axios.$post('http://localhost:3000/api/products', data);

                    this.$router.push("/");
                }
                catch (error) {
                    console.log(error);
                }
            }
        }     
    }
</script>