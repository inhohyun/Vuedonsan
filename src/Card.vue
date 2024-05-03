<template>

    <!--
        custom event -> 이걸로 부모데이터를 수정해야함(부모한테 수정해달라고 메시지를 보냄)
        $ -> Vue만의 특별한 변수 -> 부모에게 메세지 보낼 땐 $emit
        $emit('보낼 메세지명', 보낼 데이터) -> 데이터는 안 보내도 됨


    -->
  <div >
    <img :src="원룸.image" class="room-img">
    <h4 @click="send">{{ 원룸.title }}</h4>
    <p>{{  원룸.price}}원</p>
  </div>

</template>

<script>
export default {
    // eslint-disable-next-line vue/multi-word-component-names
    name: 'Card',

    props: {
        //코드가 길어지면 자료형 틀려서 생기는 버그가 많음 -> 부모로부터 가져오는 데이터 타입을 명시해주자
        원룸: Object,

    },
    //$emit()을 함수안에서 하고 싶으면? -> this.$emit()
    methods :{
        send() {
            this.$emit('openModal', this.원룸.id)
        }
    }
}
</script>

<style>

</style>