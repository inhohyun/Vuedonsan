<template>
  <!--
    Modal.vue안에 {{ 데이터 바인딩 }} 해놨는데 밑에 데이터가 없네...
    -> 컴포넌트 만들면 데이터바인딩할 때 귀찮은 일이 생길 수 있음

    {{ 데이터바인딩 }}은 밑에 데이터가 있어야 가능함
    -> 그럼 app.vue에 있는 데이터를 밑에 복붙하나? -> 뭔짓이야

    데이터는 한 곳에 보관함 -> app.vue같은곳에 넣어놓고 보관
    -> 그리고 필요하면 가져다 씀 -> props


  -->
  <!--
    유용한거 : 

    사용자의 입력을 제한하는 방식(알맞은 형식의 데이터인지)
    1. watcer를 사용해서 입력 데이터 감시하기
    2. vue 전용 form validation 라이브러리 활용하기-> watcher 사용 안하고 약간의 html 수정으로 데이터 제한 가능
  -->
  <!--모달창 ui-->
  <div class="black-bg" v-if="모달창열려있는지">
    <div class="white-bg">
      <h4>{{원룸들[누른거].title}}</h4>
      <img :src="원룸들[누른거].image" class="room-img">
      <p>{{ 원룸들[누른거].content }}</p>
      <!-- <input @input="month = $event.target.value"> -->
      <!--v-model의 목적 : 사용자의 입력 값을 변수에 저장함-->
      <!--사용자가 input에 입력한 것은 전부 문자 자료형임 js는 문자 숫자 곱셈을 숫자로 치환해서 계산, 덧셈은 이어붙임-->
      <input v-model="month">
      <p>{{ month }} 개월 선택함 :  {{ 원룸들[누른거].price * month}}원</p>
      <button @click="$emit('closeModal')"> 닫기</button>


      <!-- <button @click="모달창열려있는지=false">창 닫기</button> -->
    </div>
  </div>

</template>

<script>
export default {
    // eslint-disable-next-line vue/multi-word-component-names
    name: 'Modal',
    data() {

        return {
            month : 1,
        }
    },
    //사용자의 입력을 감시하는 함수
    watch: {
        //함수명 -> 이름이 같은 변수를 감시하는 함수, 아래는 month변수를 감시하는 함수
        //해당 변수의 값이 변경될 때마다 실행되는 함수
        //파라미터로 값을 추가하면 파라미터로 감시하는 변수 자체를 사용할 수도 있음
        //파라미터를 2개까지 입력 가능(변경전 데이터, 변경후 데이터)
        month(a) {
            if (a >= 13) {
                alert('13이하로 입력하세요')
            }
            
        }
    },
    /* 
    step2 자식은 props로 받은거 등록
    props : {데이터이름 : 자료형}

     주의 : props는 read-only임, 받아온거 수정 금지
    */
    props: {
    
        원룸들: Array,
        누른거: Number,
        모달창열려있는지: Boolean,
        
    
    
}
}   
</script>

<style>

</style>