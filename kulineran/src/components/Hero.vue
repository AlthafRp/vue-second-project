<template>
    <div class="hero">
        <!-- laptop -->
        <div class="d-none d-md-block">
            <div class="row mt-4">
                <div class="col-md-6">
                    <div class="d-flex h-100">
                        <div class="justify-content-center align-self-center">
                            <h2><strong>Best Food Ever,</strong><br> you must Try</h2>
                            <p>Try it Now</p>
                            <router-link to="/foods" class="btn btn-success">Eat Now</router-link>
                        </div>
                    </div>
                </div>
                <div class="col-md-6">
                    <img src="../assets/image/eat1.png" width="100%" />
                </div>
            </div>
        </div>
        
        <!-- handphone -->
        <div class="d-sm-block d-md-none">
            <div class="row mt-4">
                <div class="col-md-6 mb-3">
                    <img src="../assets/image/eat1.png" width="100%" />
                </div>
                <div class="col-md-6">
                    <div class="d-flex h-100">
                        <div class="justify-content-center align-self-center">
                            <h2><strong>Best Food Ever,</strong><br> you must Try</h2>
                            <p>Try it Now</p>
                            <router-link to="/foods" class="btn btn-success">Eat Now</router-link>
                        </div>
                    </div>
                </div>
                
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name
}
</script>

<style>

</style>