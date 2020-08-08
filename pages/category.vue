<template>
    <main>
        <div class= "container- fluid">
            <div class="row">
                <div class="col-sm-3"></div>
                <div class="col-sm-6">
                    <div class="a-section">
                        <div class="a-spacing-top-medium"></div>
                        <h2 style="text-align: center">Add new Category</h2>
                        <form  enctype="multipart/form-data">
                            <!-- Input -->
                            <div class="a-spacing-top-medium">
                                <label style="margin-bottom: 0px;" >Type</label>
                                <input type="text" class="a-input-text" style ="width: 100%" v-model="type">
                            </div>
                            <!-- Button -->
                            <div class="a-spacing-top-large">
                                <span class="a-button-register">
                                    <span class="a-button-inner">
                                        <span class="a-button-text" @click="onAddCategory">Add Category</span>
                                    </span>
                                </span>
                            </div>
                        </form>
                        <div class="a-spacing-top-medium"></div>
                        <ul class="list-group-item">
                            <li v-for="category in categories" :key="category._id">{{category.type}}</li>
                        </ul>
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
                type: ''
            }
        },
        async asyncData({$axios}) {
            try {
                let resp = await $axios.$get('http://localhost:3000/api/categories');
                return {
                    categories: resp.categories
                }
            } catch (err) {
                console.log(err);
            }
        },
        methods: {
            async onAddCategory() {
                try {
                    let resp = await this.$axios.$post('http://localhost:3000/api/categories', {type:this.type});
                    if (resp.success) {
                        this.categories.push({type: this.type});
                    }
                }
                catch (error) {
                    console.log(error);
                }
            }
        }
    }
</script>