<template>
    <b-nav tabs>
        <b-nav-item disabled>Fancy Quiz App</b-nav-item>
        <b-nav-item disabled>{{numCorrect}}/{{numTotal}}</b-nav-item>
    </b-nav>
</template>

<script>
export default {
    props:[
        'numCorrect' ,
        'numTotal'
    ]
}
</script>
