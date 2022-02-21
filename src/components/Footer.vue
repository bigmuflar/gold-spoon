<template>
	<div id="footer">
		<v-row id="footer__text ">
			<a href="https://github.com/bigmuflar/gold-spoon.git" target="_blank">
				GitHub
			</a>
		</v-row>
	</div>
</template>

<script>

export default {
	name: "app-footer"
};
</script>

<style lang="scss" scoped>
#footer {
	text-align: center;
	background: #112d4e;
	&__text {
		color: #f9f7f7;
		margin-bottom: 0px;
	}
	a {
		text-decoration: none;
    color: #a6e3e9;
	}
}
</style>
