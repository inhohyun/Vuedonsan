
<!--
  Vue의 라이프 사이클

  컴포넌트는 웹페이지에 표시되기까지 일련의 step을 거침
  -> 그 step을 라이프사이클이라고 함
  create 단계(데이터만 존재하는 단계) -> mount 단계(<template> 사이에 있던걸 실제 html로 바꿔줌) 
    -> 컴포넌트 단계(그 다음에 index.html에 장착함)
    -> update 단계(data가 변하면 html이 재렌더링 된다고 했음 -> 근데 html이 아니라 컴포넌트가 재렌더링 되는거임)
    -> unmount 단계(컴포넌트가 삭제되면)

    라이프사이클을 어따 써먹을까
    - 사이클 중간중간에 hook을 걸 수 있음 -> Lifecycle Hook을 걸어서 중간에 원하는 코드를 실행 가능
      mount되기전에 이것좀 실행해줘~라는 느낌
-->


<template>
  <!--
    input에 문자입력하면 경고문을 띄우고 싶다.
    -> watcher 쓰셈(data를 감시하는 함수)
  -->
  <!--
    동적인 ui 만드는 법
    0. 동적으로 만들 ui를 미리 만들기(모달창 같은거)
    1. ui의 현재 상태를 데이터로 저장해둠 : vue 코드, 모달이 열려있는지 여부 같은거 
    2. 데이터에 따라 ui가 어떻게 보일지 작성

    자식 컴포넌트가 부모가 갖고 있는 데이터를 쓰려면? - > props로 데이터를 전송해야 함
    1. 데이터를 전송
    2. 데이터를 등록
    3. 데이터를 가져다 씀
    보내는 법
    <자식 :데이터="데이터">

  
    
    클래스 이름을 동적으로 줘서 css를 애니메이션 효과로 주기
    class 명을 조건부로 넣으려면 클래스 명을 객체에 넣기 -> {클래스명 : 조건}

    -> vue에서는 <transition>을 이용하면 애미메이션을 쉽게 줄 수 있음
      1. 애니메이션을 주고 싶은 요소를 <transision name="작명">으로 감싸기
      2. class명 3개 작성
        .작명-enter-from -> 시작 상태
        .작명-enter-active -> 줄 애니메이션(transition)
        .작명-enter-to -> 끝 상태
  -->

  <transition  name="fade">
  <!-- <div class="start" :class="{ end : 모달창열려있는지 }"> -->
    <Modal @closeModal="모달창열려있는지 = false" :원룸들="원룸들" :누른거="누른거" :모달창열려있는지="모달창열려있는지"/>

  <!-- </div> -->
</transition>


  <div class="menu">
    <!-- 
      :key=""의 용도
      - 반복문 쓸 때 꼭 써야함
      - 반복문 돌린 요소를 컴퓨터가 구분하기 위해 씀 
    -->
    <a v-for="작명 in 메뉴들" :key="작명">{{작명}}</a>
  </div>


  <!--아래 html이 너무 길고 복잡하다는 전제하 html은 한 줄로 줄이고 싶음-->
  <!--축약해둔 컴포넌트 쓰는 법
    1. vue파일 import 해오고
    2. 등록하고
    3. 씀

    초보 특)
    온갖거 다 컴포넌트로 만들어둠
    반복적으로 출현할 부분만 컴포넌트화 권장 -> 컴포넌트 쓰면 필연적으로 코드가 복잡해짐
  -->

 <Discount v-if="showDiscount == true"/>

 <button @click="priceSort">가격순 정렬</button>
 <button @click="sortBack">되돌리기</button>


  <img alt="Vue logo" src="./assets/logo.png">

    <!--
      img 넣는 법
      - 절대경로는 알고있으면 그냥 넣으면 됨(http://)
      - 상대경로는 assets 폴더에 넣어서 연결

    -->
    <!--
      html 태그안의 속성 데이터바인딩은 : 
      html 태그안의 내용 데이터바인딩은 {{}}


      $event -> 자식 컴포넌트가 보낸 데이터
    -->
<Card @openModal="모달창열려있는지=true; 누른거=$event" :원룸="원룸들[i]" v-for="(작명, i) in 원룸들" :key="작명"/>

<!--
  css로 애니메이션 주려면
  1. 시작전 class 명
  2. 애니메이션이 끝난 후 class명
  3. 그리고 원할 때 2번 class명 부착

-->
<!-- <Card :원룸="원룸들[1]"/>
<Card :원룸="원룸들[2]"/>
<Card :원룸="원룸들[3]"/>
<Card :원룸="원룸들[4]"/>
<Card :원룸="원룸들[5]"/> -->





</template>

<script>
import data from './assets/oneroom.js';
import AppDiscount from "./Discount.vue";
import Modal from "./Modal.vue";
import Card from "./Card.vue";



export default {
  name: 'App',
  //vue 개발 팁 : 데이터 어떻게 만들어놓을지 생각
  data() {

    const increase = () => {
      // vue에서 함수 만들 때 주의사항
      // 함수 안에서 데이터 쓸 땐 this.데이터 명으로 쓸 것
      this.신고수++;
    }
    return {
      showDiscount : true,
      //arr/object 데이터의 각각 별개의 사본을 만들려면 [...array]
      원룸들오리지널 : [...data],
      누른거 : 0,
      원룸들 : data,
      모달창열려있는지 : false, // 모달창의 현재 상태
      신고수 : [0,0,0],
      메뉴들: ['Home', 'Shop', 'About'],


      //하드 코딩 안하고 데이터바인딩 하는 이유 -> vue의 실시간 자동 렌더링 쓰려고
      //data를 변경하면 data와 관련된 html에도 실시간으로 반영됨 -> 웹앱 같은거 만들기 좋음
      //자주 변할거 같은 데이터들은 데이터로 보관하고 htmldp {{꽂아 넣으셈}}
      //웬만하면 안 바뀔거 같은 데이터는 그냥 하드코딩

      price1: 80,
      price2: 70,
      products: ['역삼동원룸', '천호동원룸', '마포구원룸'],
      increase,
    }
  },
  methods :{
    priceSort() {
      //숫자 순으로 정렬하기
      // sort하면 원본 데이터를 훼손함 -> 요즘은 원본 데이터를 보호하는게 유행 
      this.원룸들.sort(function (a, b) {
        return a.price - b.price
      })
    },

    sortBack() {
      //주의 : 등호(=)로 array를 집어넣으면 왼쪽 오른쪽 값을 공유해주세요 임
      this.원룸들 = [...this.원룸들오리지널];
    }



  },

  // lifecycle hook -> mounted : mount 되고나서 실행시킬 -> 현재는 app.vue 컴포넌트가 mount 됐을 때 실행하도록 함
  // 서버한테 데이터를 요청하는 타이밍 잡기에도 용이함
  mounted() {
    //2초 후에 배너가 닫히도록 설계
    setTimeout(() =>{
      //배너 닫는 코드
      this.showDiscount = false;
    }, 2000)

    // 1초마다 실행되는 코드
    setInterval(() => {
      
    }, 1000);

  },
  
  components: {
    Discount: AppDiscount,
    //그냥 Discount,로도 됨 -> 이름이 같으면 자동으로 받음 -> 이걸 뭐라하더라
    Modal: Modal,
    Card : Card,
    
  }
}
</script>



<style>
body {
  margin : 0;
}
div {
  box-sizing: border-box;
}


.fade-enter-from{ 
  transform: translateY(-1000px);
}
.fade-enter-active{
  transition: all 1s;
}
.fade-enter-to{
  transform: translateY(0px);
}


.start{
  opacity: 0;
  transition: all 1s;
}
.end{
  opacity: 1;
}


.black-bg {
  width: 100%; height:100%;
  background: rgba(0,0,0,0.5);
  position: fixed; padding: 20px;
}
.white-bg {
  width: 100%; background: white;
  border-radius: 8px;
  padding: 20px;
} 



.room-img {
  width: 100%;
  margin-top: 40px;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.menu {
  background : darkslateblue;
  padding : 15px;
  border-radius : 5px;
}
.menu a {
  color : white;
  padding : 10px;
}
</style>
