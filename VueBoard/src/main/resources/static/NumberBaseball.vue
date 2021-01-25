<template>
	<div>
		<h1>{{result}}</h1>
		<form @submit.prevent="onSubmitButton">
			<input ref="answer" maxlength="4" v-model="value">
			<button type="submit">입력</button>
		</form>
		<div>시도:{{tries.length}}</div>
		<ul>
			<li v-for="t in tries" v-bind:key="t">
				<div>{{t.try}}</div>
				<div>{{t.result}}</div>
			</li>
		</ul>
	</div>
</template>

<script>
const getNumbers = () => {
	const candidates = [1, 2, 3, 4, 5, 6, 7, 8, 9];
	const array = [];
	for(let i = 0; i < 4; i++){
		const chosen = candidates.splice(Math.floor(Math.random() * (9-i)), 1)[0];
		array.push(chosen);
	}
	return array;
}

export default {
	data(){
		return {
			answer:getNumbers(),
			tries:[],
			value: '',
			result: '',
		}
	},
	methods: {
		onSubmitButton(e){
			// e.preventDefault();	//vue에서 자동으로 적용 submit.prevent
			if(this.value === this.answer.join('')){
				this.result = '홈런';
				this.value = '';
				this.tries = [];
				this.answer = getNumbers();
				this.$refs.answer.focus();
			}else{
				if(this.tries.length >= 9){
					this.result = '땡! 틀렸습니다 정답은'+this.answer.join(',');
					this.value = '';
					this.tries = [];
					this.answer = getNumbers();
					this.$refs.answer.focus();
					return;
				}
				let strike = 0;
				let ball = 0;
				const answerArray = this.value.split('').map(v => parseInt(v));
				for(let i=0; i<4; i++){
					if(answerArray[i] === this.answer[i]){
						strike++;
					}else if(this.answer.includes(answerArray[i])){
						ball++;
					}
				}
				this.tries.push({
					try:this.value,
					result:`${strike} 스트라이크, ${ball}입니다.`
				});
				this.value = '';
				this.$refs.answer.focus();
			}
			this.tries.push({
				try:this.value,
				result:	this.result
			});
			this.value = '';
			this.$refs.answer.focus();
		}
	},
}
</script>

<style>

</style>