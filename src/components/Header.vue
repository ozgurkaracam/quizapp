<template>
    <div>
        <b-nav tabs>
            <b-nav-item active>Quizz App</b-nav-item>
            <b-nav-item>Counter :4/{{ totalCounter }}</b-nav-item>
        </b-nav>
    </div>
</template>

<script>
    export default{
        data(){
            return {

            }
        },
        props:['totalCounter']
    }
</script>