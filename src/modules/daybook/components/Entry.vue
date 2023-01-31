<template>
  <div class="entry-container mb-3 pointer p-2" @click="$router.push({name: 'entry', params: {id:10}})">
    <div class="entry-title d-flex">
        <span class="text-success fs-5 fw-bold">29</span>
        <span class="mx-1 fs-5">Enero</span>
        <span class="mx-2 fw-light">2023, domingo</span>
    </div>
    <div class="entry-description">
        Laboris exercitation commodo labore tempor consequat mollit minim est sit. Consequat excepteur sint fugiat irure deserunt. Incididunt est quis excepteur cupidatat. Aute elit tempor anim proident sunt veniam. Laborum nulla labore deserunt voluptate aliquip esse veniam proident.
    </div>
  </div>
</template>

<script>
export default {

}
</script>

<style lang="scss" scoped>
.entry-container{
    border-bottom: 1px solid #2c3e50;
    transition: 0.2s all ease-in;
    &:hover{
        background-color: lighten($color: grey, $amount: 45);
        transition: 0.2s all ease-in;
    }
    .entry-description{
    font-size: 12px;
    }
}

</style>