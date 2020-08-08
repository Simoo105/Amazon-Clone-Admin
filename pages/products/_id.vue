<template>
    <main>
        <div class= "container- fluid">
            <div class="row">
                <div class="col-sm-3"></div>
                <div class="col-sm-6">
                    <div class="a-section">
                        <div class="a-spacing-top-medium"></div>
                        <h2 style="text-align: center">Update {{product.title}}</h2>
                        <form  enctype="multipart/form-data">
                            <!-- Category Dropdown -->
                            <div class="a-spacing-top-medium">
                                <label >Category</label>
                                <select class="a-select-option" v-model="product.category._id" name="" id="">
                                    <option v-for="category in Allcategories" :key="category._id" :value="category._id">{{category.type}}</option>
                                </select>
                            </div>
                            <!-- Owner Dropdown -->
                            <div class="a-spacing-top-medium">
                                <label >Owner</label>
                                <select class="a-select-option" v-model="product.owner._id" name="" id="">
                                    <option v-for="owner in Allowners" :key="owner._id" :value="owner._id">{{owner.name}}</option>
                                </select>
                            </div>
                            <!-- Title Input -->
                            <div class="a-spacing-top-medium">
                                <label style="margin-bottom: 0px;" >Title</label>
                                <input type="text" class="a-input-text" style ="width: 100%" v-model="product.title" :placeholder="product.title">
                            </div>
                            <!-- Price Input -->
                            <div class="a-spacing-top-medium">
                                <label style="margin-bottom: 0px;" >Price</label>
                                <input type="number" class="a-input-text" style ="width: 100%" v-model="product.price" :placeholder="product.price">
                            </div>
                            <!-- stockQuantity Input -->
                            <div class="a-spacing-top-medium">
                                <label style="margin-bottom: 0px;" >Stock Quantity</label>
                                <input type="number" class="a-input-text" style ="width: 100%" v-model="product.stockQuantity" :placeholder="product.stockQuantity">
                            </div>
                            <!-- Description Input -->
                            <div class="a-spacing-top-medium">
                                <label style="margin-bottom: 0px;" >Description</label>
                                <textarea :placeholder="product.description" name="" style="width: 100%" v-model="product.description" ></textarea>
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
                                        <span class="a-button-text" @click="onUpdateProduct">Update product</span>
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
                selectedFile: null,
                fileName: ''
            }
        },
        async asyncData({$axios, params}) {
            try {
                let categories = $axios.$get('http://localhost:3000/api/categories');
                let owners = $axios.$get('http://localhost:3000/api/owners');
                let Product = $axios.$get(`http://localhost:3000/api/products/${params.id}`);

                const [catResponse, ownerResponse, proResponse] = await Promise.all([categories, owners, Product]);
                return {
                    Allcategories: catResponse.categories,
                    Allowners: ownerResponse.owners,
                    product: proResponse.product
                }
            }
            catch (error) {
                console.log(error)
            }
        },
        methods: {
            onFileSelected(event) {
                this.selectedFile = event.target.files[0];
                this.fileName = event.target.files[0].name;
            },
            async onUpdateProduct() {
                let data = new FormData();
                data.append("title", this.product.title);
                data.append("price", this.product.price);
                data.append("description", this.product.description);
                data.append("ownerID", this.product.owner._id);
                data.append("stockQuantity", this.product.stockQuantity);
                data.append("categoryID", this.product.category._id);
                data.append("photo", this.selectedFile, this.selectedFile.name);

                try {
                    let result = await this.$axios.$put(`http://localhost:3000/api/products/${this.$route.params.id}`, data);

                    if(!result.success) {
                        console.log(result.message);
                    }
                    
                    this.$router.push("/");
                }
                catch (error) {
                    console.log(error);
                }
            }
        }     
    }
</script>