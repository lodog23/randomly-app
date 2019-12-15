<template>
  <div class="shapeContainer">

<div class="firstwave">
  <div class="blueSquare"></div>
  <div class="redCircle"></div>
  <div class="yellowCircle"></div>
  <div class="pinkSquare"></div>
  <div class="blueCircle"></div>
  <div class="redCircle2"></div>
  <div class="greenCircle"></div>
  <div class="pinkSquare2"></div>
  <div class="purpleCircle"></div>
  <div class="blueCircle2"></div>
  <div class="yellowSquare"></div>
  <div class="greenCircle2"></div>
</div>

 <div class="secondwave">
   <div class="blueSquare"></div>
<div class="redCircle"></div>
<div class="yellowCircle"></div>
<div class="pinkSquare"></div>
<div class="blueCircle"></div>
<div class="redCircle2"></div>
<div class="greenCircle"></div>
<div class="pinkSquare2"></div>
<div class="purpleCircle"></div>
<div class="blueCircle2"></div>
<div class="yellowSquare"></div>
<div class="greenCircle2"></div>
</div>

</div>
</template>

<script>
export default {
  name: 'ShapeAnimation',
  props: {

  }
}
</script>

<style scoped lang="scss">

.shapeContainer {
  width: 250px;
  height: 200px;
  position: absolute;
  overflow: hidden;
  top: -140px;
}

@keyframes float {
	0% {
		transform: translatey(0px) scale(1.0);
    opacity: 100%;
	}
	50% {
	opacity: 100%;
    transform: translatey(-125px) scale(1.0);
	}

	70% {
	opacity: 100%;
	}

  98% {
    opacity: 100%;
  }

  99% {
    opacity: 100%;
  }

	100% {
		transform: translatey(-250px) scale(0);
    // opacity: 0;
	}
}

@keyframes float2 {
	0% {
		transform: translatey(0px);
	}
	100% {
		transform: translatey(-400px);
	}
}

.blueSquare {
  position: absolute;
  width: 15.77px;
  height: 15.77px;
  background: #00C2FF;
  border-radius: 2px;
  transform: rotate(-47.24deg);
/*   bottom: 3px; */
    bottom: -97px;

  left: 1px;
/*   transform: translatey(0px); */
	animation: float 6s infinite;
}

.redCircle {
  position: absolute;
  width: 8.01px;
  height: 8.01px;
  background: #FF0050;
  transform: rotate(2.05deg);
  border-radius: 50px;
/*   bottom: 51px; */
    bottom: -49px;

  left: 6px;
  animation: float 6s infinite;
}

.yellowCircle {
  position: absolute;
width: 13.34px;
height: 13.34px;

background: #FFE500;
transform: rotate(2.05deg);
 border-radius: 50px;
/*   bottom: 76px; */
    bottom: -24px;

  left: 27px;
  animation: float 6s infinite;
}

.pinkSquare {
  position: absolute;
  width: 15.77px;
height: 15.77px;

background: #FE00EA;
border-radius: 2px;
transform: rotate(-47.24deg);
/*   bottom: 43px;   */
  bottom: -57px;

  left: 45px;
  animation: float 6s infinite;
}

.blueCircle {
  position: absolute;
width: 25.35px;
height: 25.35px;

background: #00C2FF;
transform: rotate(2.05deg);
  border-radius: 100px;
/*   bottom: 63px; */
    bottom: -37px;

  left: 76px;
  animation: float 6s infinite;
}

.redCircle2 {
  position: absolute;
width: 8.01px;
height: 8.01px;

background: #FF0050;
transform: rotate(2.05deg);
 border-radius: 50px;
/*   bottom: 91px; */
    bottom: -9px;

  left: 98px;
  animation: float 6s infinite;
}

.greenCircle {
  position: absolute;
width: 14.68px;
height: 14.68px;

background: #01FF8D;
transform: rotate(2.05deg);
  border-radius: 50px;
/*   bottom: 26px; */
    bottom: -74px;

  left: 105px;
  animation: float 6s infinite;
}

.pinkSquare2 {
  position: absolute;
width: 12.48px;
height: 12.48px;

background: #FE00EA;
border-radius: 1px;
transform: rotate(-47.24deg);
/*   bottom: 52px; */
    bottom: -48px;

  left: 142px;
  animation: float 6s infinite;
}

.purpleCircle {
  position: absolute;
width: 21.35px;
height: 21.35px;

background: #8400FF;
transform: rotate(2.05deg);
  border-radius: 100px;
/*   bottom: 25px; */
    bottom: -75px;

  left: 159px;
  animation: float 6s infinite;
}

.blueCircle2 {
  position: absolute;
width: 13.34px;
height: 13.34px;

background: #00C2FF;
transform: rotate(2.05deg);
  border-radius: 50px;
/*   bottom: 83px; */
    bottom: -17px;

  left: 162px;
  animation: float 6s infinite;
}

.yellowSquare {
  position: absolute;
  width: 19.85px;
  height: 19.85px;
  background: #FFE500;
  border-radius: 2px;
  transform: rotate(-47.24deg);
/*   bottom: 48px; */
    bottom: -52px;

  left: 194px;
  animation: float 6s infinite;
}

.greenCircle2 {
  position: absolute;
width: 12.01px;
height: 12.01px;

background: #01FF8D;
transform: rotate(2.05deg);
  border-radius: 50px;
/*   bottom: 18px; */
    bottom: -82px;

  left: 236px;
  animation: float 6s infinite;
}

.shapeContainer > div {
  animation-timing-function: linear;
}

.firstwave {
  div {
    animation: float 8s infinite;
    animation-timing-function: linear;
  }
}

.secondwave {
  position: relative;
  // bottom: -100px;
  bottom: -200px;

}

.secondwave div {
  animation: float 8s infinite;
  animation-delay: 4s;
  animation-timing-function: linear;
}

</style>
