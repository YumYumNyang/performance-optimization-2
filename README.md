# lecture -2

## 학습할 최적화 기법

### CSS 애니메이션 최적화

브라우저가 어떻게 화면을 그리는지 학습하고 버벅거리는 애니메이션의 성능을 최적화 할 수 있다.

### 컴포넌트 지연 로딩

단일 컴포넌트를 분할하여 컴포넌트가 쓰이는 순간에 불로오도록 하여 최적화 할 수 있다.

### 컴포넌트 사전 로딩

컴포넌트 코드를 분할하여 지연 로딩을 적용하면, 첫 화면 진입 시 분할된 코드 중 당장 필요한 코드만 다운로드하기 때문에 첫화면을 더 빠르게 그릴 수 있게 된다.

하지만 분할된 컴포넌트를 사용하려고 할 때, 다운로드되어 있지 않은 코드를 추가로 다운로드하는 시간만큼 서비스 이용에 지연이 발생한다.

이런 문제를 해결하기 위해 코드를 분할하여 첫 화면 진입 시에는 다운로드하지 않지만 코드가 필요한 시점보다 더 빠르게 코드를 로드해 지연없이 사용할 수 있게 할 수 있다.

### 이미지 사전 로딩

이미지를 필요한 시점에 로드하면 이미지가 로드되는 시간만큼 기다려야한다,. 그래서 필요한 시점보다 먼저 다운로드해두고, 필요할 때 바로 이미지를 보여줄 수 있도록 하는 이미지 사전 로딩 기법을 적용해볼 수 있다.

## 코드 분석

```jsx
import React, { useState } from "react";
import styled from "styled-components";
import Header from "./components/Header";
import InfoTable from "./components/InfoTable";
import SurveyChart from "./components/SurveyChart";
import Footer from "./components/Footer";
import ImageModal from "./components/ImageModal";

function App() {
	const [showModal, setShowModal] = useState(false);

	return (
		<div className="App">
			<Header />
			<InfoTable />
			<ButtonModal
				onClick={() => {
					setShowModal(true);
				}}>
				올림픽 사진 보기
			</ButtonModal>
			<SurveyChart />
			<Footer />
			{showModal ? (
				<ImageModal
					closeModal={() => {
						setShowModal(false);
					}}
				/>
			) : null}
		</div>
	);
}

const ButtonModal = styled.button`
	border-radius: 30px;
	border: 1px solid #999;
	padding: 12px 30px;
	background: none;
	font-size: 1.1em;
	color: #555;
	outline: none;
	cursor: pointer;
`;

export default App;
```

App 컴포넌트의 코드는 위와 같다. showModal이 true값을 가지게 되면 ImageModal이 화면이 보이게 된다.

```jsx
import React from "react";
import styled from "styled-components";
import ImageGallery from "react-image-gallery";
import "react-image-gallery/styles/css/image-gallery.css";
import btnClose from "../assets/btn-close.png";

const ImageModal = (props) => {
	const images = [
		{
			original:
				"https://stillmed.olympic.org/media/Photos/2016/08/20/part-1/20-08-2016-Football-Men-01.jpg?interpolation=lanczos-none&resize=*:800",
			thumbnail:
				"https://stillmed.olympic.org/media/Photos/2016/08/20/part-1/20-08-2016-Football-Men-01.jpg?interpolation=lanczos-none&resize=*:150",
		},
		{
			original:
				"https://stillmed.olympic.org/media/Photos/2016/08/12/12-08-2016-archery-individual-men-03.jpg?interpolation=lanczos-none&resize=*:800",
			thumbnail:
				"https://stillmed.olympic.org/media/Photos/2016/08/12/12-08-2016-archery-individual-men-03.jpg?interpolation=lanczos-none&resize=*:150",
		},
		{
			original:
				"https://stillmed.olympic.org/media/Photos/2016/08/20/part-1/20-08-2016-Football-Men-02.jpg?interpolation=lanczos-none&resize=*:800",
			thumbnail:
				"https://stillmed.olympic.org/media/Photos/2016/08/20/part-1/20-08-2016-Football-Men-02.jpg?interpolation=lanczos-none&resize=*:150",
		},
		{
			original:
				"https://stillmed.olympic.org/media/Photos/2016/08/20/part-2/20-08-2016-Golf-Women-02.jpg?interpolation=lanczos-none&resize=*:800",
			thumbnail:
				"https://stillmed.olympic.org/media/Photos/2016/08/20/part-2/20-08-2016-Golf-Women-02.jpg?interpolation=lanczos-none&resize=*:150",
		},
		{
			original:
				"https://stillmed.olympic.org/media/Photos/2016/08/14/part-1/14-08-2016-Golf-Individual-Stroke-Play-Men-05.jpg?interpolation=lanczos-none&resize=*:800",
			thumbnail:
				"https://stillmed.olympic.org/media/Photos/2016/08/14/part-1/14-08-2016-Golf-Individual-Stroke-Play-Men-05.jpg?interpolation=lanczos-none&resize=*:150",
		},
		{
			original:
				"https://stillmed.olympic.org/media/Photos/2016/08/12/12-08-2016-archery-individual-men-02.jpg?interpolation=lanczos-none&resize=*:800",
			thumbnail:
				"https://stillmed.olympic.org/media/Photos/2016/08/12/12-08-2016-archery-individual-men-02.jpg?interpolation=lanczos-none&resize=*:150",
		},
		{
			original:
				"https://stillmed.olympic.org/media/Photos/2016/08/12/12-08-2016-archery-individual-men-01.jpg?interpolation=lanczos-none&resize=*:800",
			thumbnail:
				"https://stillmed.olympic.org/media/Photos/2016/08/12/12-08-2016-archery-individual-men-01.jpg?interpolation=lanczos-none&resize=*:150",
		},
		{
			original:
				"https://stillmed.olympic.org/media/Photos/2016/08/20/part-1/20-08-2016-Football-Men-03.jpg?interpolation=lanczos-none&resize=*:800",
			thumbnail:
				"https://stillmed.olympic.org/media/Photos/2016/08/20/part-1/20-08-2016-Football-Men-03.jpg?interpolation=lanczos-none&resize=*:150",
		},
	];

	return (
		<ImageModalWrapper>
			<ImageModalContainer>
				<BtnClose src={btnClose} onClick={props.closeModal} />
				<ModalHeader>올림픽 사진</ModalHeader>
				<Modalbody>
					<ImageGallery items={images} />
				</Modalbody>
			</ImageModalContainer>
		</ImageModalWrapper>
	);
};

export default ImageModal;
```

ImageModal 컴포넌트의 코드이다.

1. react-iamge-gallery 외부 라이브러리 사용 - 외부 라이브버리를 사용하면 그만큼 최종 번들링된자바스크립트의 사이즈도 커짐. 이는 서비스의 자바스크립트를 로드하는 데 시간이 오래 걸림을 의미.
2. 라이브러리에 이미지 데이터를 넘겨 화면에 표시 - 이미지의 경우 이미지 자체를 로드하는 데 시간이 걸려 사용자에게 늦게 보일 수 있고, 더 중요한 리소스를 로드하는 것을 방해할 수 있으므로 유의하여야 함.

```jsx
import React from "react";
import styled from "styled-components";

const Bar = (props) => {
	return (
		<BarWrapper
			onClick={props.handleClickBar}
			isSelected={props.isSelected}>
			<BarInfo>
				<Percent>{props.percent}%</Percent>
				<ItemVaue>{props.itemValue}</ItemVaue>
				<Count>{props.count}</Count>
			</BarInfo>
			<BarGraph
				width={props.percent}
				isSelected={props.isSelected}></BarGraph>
		</BarWrapper>
	);
};

const BarGraph = styled.div`
	position: absolute;
	left: 0;
	top: 0;
	width: ${({ width }) => width}%;
	transition: width 1.5s ease;
	height: 100%;
	background: ${({ isSelected }) =>
		isSelected ? "rgba(126, 198, 81, 0.7)" : "rgb(198, 198, 198)"};
	z-index: 1;
`;

export default Bar;
```

Bar 컴포넌트의 코드이다. Bar 컴포넌트에는 막대 그래프와 텍스트를 그리기 위한 요소가 있다.

styled-component를 사용해서 bargraph의 width속성과 transition속성으로 막대 그래프의 가로길이를 조절한다.

## 애니메이션 최적화

### 문제의 애니메이션 찾기

<aside>
💡 jank - 애니메이션이 끊기는 현상

</aside>

### 애니메이션의 원리

여러 장의 이미지를 빠르게 전환하여 눈에 잔상을 남겨 이미지가 움직이는 것처럼 느껴지게 하는 것을 말한다.

일반적인 디스플레이의 주사율은 60Hz로, 1초에 60장의 정지된 화면을 빠르게 보여줄 수 있다. 브라우저도 이에 맞춰 최대 60FPS로 1초에 60장의 화면을 새로 그린다.

CPU가 만약 다른 일을 하느라 바쁘면 초당 60장의 화면을 그리지 못해서 이런 쟁크 현상이 나타나는 것이다.

### 브라우저 렌더링 과정

주요 렌더링 경로 또는 픽셀 파이프라인이라고도 한다.

1. HTML 파싱 후 생성된 DOM트리, CSS 파싱 후 생성된 CSSOM트리
2. 렌더 트리는 DOM과 CSSOM 결합으로 생성됨 (display:none 은 렌더트리에 포함되지 않음)
3. 화면 구성 요소의 위치나 크기를 계산하는 레이아웃 과정
4. 화면 구성 요소에 색을 채워너는 작업인 페인트 과정. 여러 개의 레이어를 나눠서 작업하기도 함
5. 각 레이어를 하나로 합성하는 컴포지트 과정

![Untitled](./img/Untitled.png)

performance 탭으로 확인하면 브라우저가 어떻게 렌더링되는지 확인할 수 있다. 회색 선은

### 리플로우와 리페인트

예를 들어 요소의 사이즈가 변경되면 DOM과 CSSOM이 다시 만들어지고 렌더트리 생성부터 레이아웃까지 다시 만들어지게 되므로 브라우저 리소스를 많이 사용하게 된다.

레이아웃 관련 속성이 아니라 색상 관련 속성이 변경되면 CSSOM이 새로 생성될 것이고, 렌더트리도 새로 만들어진다. 하지만 레이아웃 단계를 건너뛰므로 리플로우 작업보다는 빠르다.

리플로우와 리페인트는 기본적으로 거의 모든 렌더링 단계를 거치기 때문에 리소스를 많이 잡아 먹는다. 이를 피하는 방법은 transform과 opacity 같은 속성을 사용하는 방법이다.

해당 요소를 별도의 레이어로 분리하고 작업을 GPU에 위임하여 처리함으로써 두 단계를 건너뛸 수 있다. 이를 하드웨어 가속이라고 한다.

-   리플로우를 발생시키는 속성: position, display ,width, float, height, font-family, top, left, font-size, font-weight, line-height, min-height , margin, padding, border
-   리페인트를 발생시키는 속성: background, background-image, background-position, border-radius, border-stylte, color, line-style,outline

### 하드웨어 가속(GPU 가속)

CPU에서 처리해야할 작업을 GPU에 위임하여 더욱 효율적으로 처리할 수 있다.

![Untitled](./img/Untitled%201.png)

### 애니메이션 최적화

width로 되어있는 애니메이션 transform으로 변경하여 최적화해보자.

```jsx
const BarGraph = styled.div`
	position: absolute;
	left: 0;
	top: 0;
	width: 100%;
	transform: scaleX(${({ width }) => width / 100});
	transform-origin: center left;
	transition: transform 1.5s ease;
	height: 100%;
	background: ${({ isSelected }) =>
		isSelected ? "rgba(126, 198, 81, 0.7)" : "rgb(198, 198, 198)"};
	z-index: 1;
`;
```

![Untitled](./img/Untitled%202.png)

확실히 초록색 부분의 바가 표현하는 paint하는 시점이 빨라진 것을 확인할 수 있다.

## 컴포넌트 지연 로딩

### 번들 파일 분석

cra-bundle-analyzer로 번들 파일을 분석할 수 있다.

```bash
npm install --save-dev cra-bundle-analyzer
npx cra-bundle-aynalyzer
```

![Untitled](./img/Untitled%203.png)

사실상 react-image-gallery는 첫 화면부터 필요하지 않기 때문에 갤러리가 있는 모달 창을 띄우는 시점에 불러와도 좋겠다는 판단이 된다.

### 모달 코드 분리하기

```jsx
// import ImageModal from "./components/ImageModal";
const LazyImageModal = lazy(() => import("./components/ImageModal"));
function App() {
	const [showModal, setShowModal] = useState(false);

	return (
		<div className="App">
			<Header />
			<InfoTable />
			<ButtonModal
				onClick={() => {
					setShowModal(true);
				}}>
				올림픽 사진 보기
			</ButtonModal>
			<SurveyChart />
			<Footer />
			<Suspense fallback={null}>
				{showModal ? (
					<LazyImageModal
						closeModal={() => {
							setShowModal(false);
						}}
					/>
				) : null}
			</Suspense>
		</div>
	);
}
```

위와 같이 imageModal을 lazyload하고 Suspense로 감싸주었다.

![Untitled](./img/Untitled%204.png)

모달 보는 버튼을 누르고 난 이후에 2개의 코드가 로드되는 것을 확인할 수 있다.

![Untitled](./img/Untitled%205.png)

하나의 번들의 크기를 줄일 수 있었다.

## 컴포넌트 사전 로딩

### 지연 로딩의 단점

이런 방법은 초기 화면 로딩에는 효과적이지만, 모달을 띄울 때 지연시간이 발생하는 문제가 있다.

performance 패널에서 Fast3G로 설정 후 페이지가 모두 로드된 상태에서 기록을 하였을 때 모달 튼을 누른 시점과 모달이 뜨는 시점이 차이가 남을 확인할 수 있다.

![Untitled](./img/Untitled%206.png)

이런 문제를 사전 로딩(preloading) 기법을 이용하여 필요한 모듈을 필요해지기 전에 미리 로드할 수 있다. 이 모달 컴포넌트의 경우, 모달을 보여주는 버튼에 마우스가 오버되었을 때 사전 로딩을 하도록 만들 수 있다.

```jsx
const handleMouseEnter = () => {
	const component = import("./components/ImageModal");
};
```

또는 컴포넌트가 마운트가 완료되었을 때 사전 로딩을 수행할 수 있다.

```jsx
useEffect(() => {
	const component = import("./components/ImageModal");
}, []);
```

## 이미지 사전 로딩

### 느린 이미지 로딩

![Untitled](./img/Untitled%207.png)

이미지가 제때 뜨지 않아 생기는 현상이다. 이때 이미지가 화면에 제때 뜰 수 있도록 미리 다운로드 하는 기법인 이미지 사전 로딩 기법을 적용해 볼 수 있다.

### 이미지 사전 로딩

컴포넌트는 import 함수를 이용하여 로드하는데, 이미지는 이미지가 화면에 그려지는 시점에 로드된다. 또는 자바스크립트로 이미지를 직접 로드하는 방법이 있는데, 바로 Image 객체를 사용하는 법이다. new 연산자를 이용하여 생성할 수 있고, src 속성에 원하는 이미지의 주소를 입력하면 해당 이미지를 로드할 수 있다.

```jsx
const img = new Image();
img.src = "{이미지 주소}";
```

```jsx
useEffect(() => {
	const img = new Image();
	img.src =
		"https://stillmed.olympic.org/media/Photos/2016/08/20/part-1/20-08-2016-Football-Men-01.jpg?interpolation=lanczos-none&resize=*:800";
	const component = import("./components/ImageModal");
}, []);
```

이렇게 사전 로딩을 하게 한 후에 네트워크 패널을 살펴보면, App 컴포넌트가 렌더링이 된 이후에 이미지를 사전 로딩해오는 것을 확인할 수 있다.

![Untitled](./img/Untitled%208.png)

그리고 모달을 열게 되면 이미 로드되어 disk cache에 있는 이미지를 불러오는 것을 확인할 수 있다.

![Untitled](./img/Untitled%209.png)

이때 몇 장의 이미지까지 사전로드를 해 둘 것인지 잘 생각하고 판단해서 사전로딩을 하는 것이 좋다. 사전 로딩을 너무 많이 하는 경우에 브라우저 리소스를 많이 사용하는 것이므로 다른 성능 문제를 야기할 수 있기 때문이다.
