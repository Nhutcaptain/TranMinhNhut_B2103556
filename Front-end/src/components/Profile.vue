<script>
    export default{
        props: {
            user: {type: Object, required: true},
        },
    }
</script>

<template>
    <div class="user-profile">
        <div class="p-1">
            <strong>Tên: </strong>
            {{ user.name }}
        </div>
        <div class="p-1">
            <strong>Email: </strong>
            {{ user.email }}
        </div>
        <div class="p-1">
            <strong>Địa chỉ: </strong>
            {{ user.address }}
        </div>
        <div class="p-1">
            <strong>Số điện thoại: </strong>
            {{ user.phone }}
        </div>
    </div>
</template>