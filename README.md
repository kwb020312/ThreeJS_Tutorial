What is ThreeJS ?

Three.js는 웹 브라우저에서 애니메이션 3차원 컴퓨터 그래픽스를 만들고 표시하기 위해 사용되는 크로스 브라우저 자바스크립트 라이브러리이자 API이다. (출처 : https://ko.wikipedia.org/wiki/Three.js)

<img src="gitImages\threeJS_Logo.png">

본 저장소는 three.js 의 공식 문서를 참고하여 작성되었음 을 밝힘
출처 : https://threejs.org/docs/index.html#manual/ko/introduction

## 설치

```javascript
// npm 사용시
npm install --save three
```

```html
<!-- cdn 사용시 -->
<script type="module">
    import * as THREE from 'https://unpkg.com/three/build/three.module.js';
</script>
```

참고로 three.js 는 웹을 목적으로 만들어졌고 , import 문을 사용하지만 Node.js 특성상 require 를 사용해야 하기 때문에, Node.js 사용을 지양하고 있다.

## 초기 작업

화면에 3D 그래픽을 삽입하기 위해서는 필수 작업이 필요한데,

```javascript
// 화면에 넣는다는 것을 선언
const scene = new THREE.Scene();
/* 
    어느 시점에서 볼 것인지를 선언하는데 , 
    첫 번째 인자는 얼마나 보여줄지
    두 번째 인자는 종횡비
    세 번째 인자 와 네 번째 인자는 얼마나 가까이 볼 수 있는가 와 얼마나 떨어져 볼 수 있는가 를 설정하는 것 이다.
*/
const camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );
// 렌더링을 도와줄 도구를 선언하고
const renderer = new THREE.WebGLRenderer();
// 크기조정 세 번째 인자로 false 를 전달하면 절반의 해상도로 렌더링 됨
renderer.setSize( window.innerWidth, window.innerHeight );
document.body.appendChild( renderer.domElement );
```

## 큐브 삽입

```javascript
// 큐브에 필요한 모든 꼭짓점 데이터 와 면 데이터 가 포함되어있는 객체
const geometry = new THREE.BoxGeometry();
// 색상을 초록색 으로 설정함
const material = new THREE.MeshBasicMaterial( { color: 0x00ff00 } );
// cube 변수에 모든 설정을 입힘
const cube = new THREE.Mesh( geometry, material );
// 화면에 추가
scene.add( cube );

// z-index 가 5임
camera.position.z = 5;
```

## 움직이기

```javascript
const animate = function () {
    // 재귀함수의 개념이며 초당 60회 호출된다.
    requestAnimationFrame( animate );

    // x 의 값과 y 의 값을 0.01 만큼 씩 추가함
    cube.rotation.x += 0.01;
    cube.rotation.y += 0.01;

    // 해당 목표를 화면에 그려줌
    renderer.render( scene, camera );
};

animate();
```

모두 완료 시 

<img src="gitImages\turn_square.png">

위와 같은 사각형 큐브가 회전한다.

## 선 만들기

```javascript
// 모양을 선 으로 선언함
const material = new THREE.LineBasicMaterial( { color: 0x0000ff } );

// 꼭짓점 좌표에 대한 설정
const points = [];
points.push( new THREE.Vector3( -10, 0, 0 ) );
points.push( new THREE.Vector3( 0, 10, 0 ) );
points.push( new THREE.Vector3( 10, 0, 0 ) );
points.push( new THREE.Vector3( 20, 10, 0 ) );
const geometry = new THREE.BufferGeometry().setFromPoints( points );

// 선 객체를 생성함
const line = new THREE.Line( geometry, material );
```

모두 완료 시 

<img src="gitImages\lines.png">

해당 선이 나온다.