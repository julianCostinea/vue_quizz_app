<template>
    <div>
        <b-nav tabs>
            <b-nav-item disabled><b>Home</b></b-nav-item>
            <b-nav-item disabled>Counter: {{ numCorrect }}/{{ numTotal }}</b-nav-item>
        </b-nav>
    </div>
</template>

<script>
export default {
    name: 'HeaderComponent',
    props: {
        numCorrect: Number,
        numTotal: Number
    }
}
</script>