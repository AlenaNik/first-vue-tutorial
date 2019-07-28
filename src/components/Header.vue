<template>
    <div>
        <b-nav tabs fill>
            <b-nav-item disabled><b>Video Games Quiz</b></b-nav-item>
            <b-nav-item disabled>Counter: {{ numCorrect }} / {{ numTotal }}</b-nav-item>
        </b-nav>
    </div>
</template>

<script>
    export default {
        props: [
            'numCorrect',
            'numTotal'
        ]
    }
</script>