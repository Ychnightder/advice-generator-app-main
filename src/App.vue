<script setup>
import { onMounted, ref, onUnmounted  } from 'vue';

const img = ref('');

const id = ref(0);
const text = ref('');





const getAdvice = async () => {
	const res = await fetch('https://api.adviceslip.com/advice');
	const data = await res.json();
	id.value = data.slip.id;
	text.value = data.slip.advice;
};

const updateImage = () => {
	const isMobile = window.innerWidth < 768;
	img.value = isMobile 
	? '../public/assets/images/pattern-divider-mobile.svg' 
	: '../public/assets/images/pattern-divider-desktop.svg';
};
onMounted(() => {
	updateImage(); // Initialisation de l'image
	window.addEventListener('resize', updateImage);
	getAdvice(); // Appel de la fonction pour récupérer l'avis
});
onUnmounted(() => {
  window.removeEventListener('resize', updateImage); // Nettoyage
});

</script>

<template>
	<main class="main">
		<h3 class="id-advice">Advice # {{ id }}</h3>
		<p class="text-advice">{{ text }}</p>
		<div class="wrapper-img">
			<img class="fix-img" :src="img" alt="ff" />
		</div>

		<button class="dice" @click.prevent="getAdvice()" >
			<svg width="24" height="24" xmlns="http://www.w3.org/2000/svg">
				<path
					d="M20 0H4a4.005 4.005 0 0 0-4 4v16a4.005 4.005 0 0 0 4 4h16a4.005 4.005 0 0 0 4-4V4a4.005 4.005 0 0 0-4-4ZM7.5 18a1.5 1.5 0 1 1 0-3 1.5 1.5 0 0 1 0 3Zm0-9a1.5 1.5 0 1 1 0-3 1.5 1.5 0 0 1 0 3Zm4.5 4.5a1.5 1.5 0 1 1 0-3 1.5 1.5 0 0 1 0 3Zm4.5 4.5a1.5 1.5 0 1 1 0-3 1.5 1.5 0 0 1 0 3Zm0-9a1.5 1.5 0 1 1 0-3 1.5 1.5 0 0 1 0 3Z"
					fill="#202733" /></svg
		></button>
	</main>
</template>

<style lang="scss">
@import './scss/style.scss';
.main {
	display: flex;
	flex-direction: column;
	align-items: center;
	background-color: $Blue900;
	width: 570px;
	max-height: 100%;
	max-width: 100%;
	height: 35%;
	border-radius: 15px;
	transition: all 0.3s ease-in-out;
	text-align: center;
	position: relative;

	.id-advice {
		width: 100%;
		color: $Green300;
		font-weight: 700;
		letter-spacing: 4px;
		text-transform: uppercase;
		font-size: 14px;
		margin-top: 50px;
		margin-bottom: 25px;
	}

	.text-advice {
		font-size: 25px;
		font-weight: 800;
		line-height: 1.5;
		margin-bottom: 30px;
		width: 90%;
	}
	.wrapper-img {
		width: 100%;
		max-width: 100%;
		max-height: 100%;
		object-fit: cover;
		border-radius: 15px;
	}
	.dice {
		z-index: 2;
		position: absolute;
		bottom: -10%;
		background-color: $Green300;
		width: 60px;
		height: 60px;
		border-radius: 50%;
		display: flex;
		justify-content: center;
		align-items: center;
		margin-top: 30px;
		text-decoration: none;
		color: $Blue900;
		transition: all 0.3s ease-in-out;
		cursor: pointer;

		&:hover {
			background-color: $Green300;
			box-shadow: 1px 1px 20px $Green300;
		}
	}
}

@media screen and (max-width: 768px) {
	.main {
		width: 75%;
		height: auto;

		.text-advice {
			font-size: 19px;
		}

		.text-advice {
			margin-bottom: 20px;
		}
		.wrapper-img {
			margin-bottom: 70px;
		}
		
	}
}
</style>
