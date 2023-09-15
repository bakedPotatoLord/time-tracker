<template>
	<div class="container">
		<div class="topStuff">

			<Top />
			<TopDivider/>
		</div>
		<div class="router-container">
			<NuxtPage />
		</div>
	</div>
</template>

<style scoped>
.topStuff {
	position:fixed;
	top:0;
	left:0;
	width:100%;
}
.router-container {
	margin-top: 150px;
	margin-left: 1rem;
	margin-right: 1rem;
	background:linear-gradient(90deg,#66289c,#201d38)
}
</style>

<style >
@import url('https://fonts.googleapis.com/css2?family=Fira+Sans:ital,wght@0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,300;1,400;1,500;1,600;1,700;1,800&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@500&display=swap');

body {
	margin: 0;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	background: linear-gradient(90deg,#66289c,#201d38);
	font-family: 'Montserrat', sans-serif;

	color:rgb(226, 226, 226)
}
input::placeholder,
textarea::placeholder {
	color:rgb(168, 168, 168)
}
</style>
