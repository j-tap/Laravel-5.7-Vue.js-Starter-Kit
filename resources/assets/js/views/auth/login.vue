<template lang="pug">
#wrapper
	.login-register
		.login-box.card
			.card-body
				form#loginform.form-horizontal.form-material(@submit.prevent='submit')
					h3.box-title.m-b-20 Sign In
					.form-group
						.col-xs-12
							input.form-control(
								v-model='loginForm.email'
								placeholder='Email'
								name='email'
								type='text'
							)
					.form-group
						.col-xs-12
							input.form-control(
								v-model='loginForm.password'
								placeholder='Password'
								name='password'
								type='password'
							)
					.form-group.text-center.m-t-20
						.col-xs-12
							button.btn.btn-info.btn-lg.btn-block.text-uppercase.waves-effect.waves-light(
								type='submit'
							) Log In
					.row
						.col-xs-12.col-sm-12.col-md-12.m-t-10.text-center
							.social
								a.btn.btn-github.mr-1(
									href='/auth/social/github'
									data-toggle='tooltip'
									title='Login with Github'
								)
									i.fa.fa-github(aria-hidden='true')

								a.btn.btn-twitter.mr-1(
									href='/auth/social/twitter'
									data-toggle='tooltip'
									title='Login with Twitter'
								)
									i.fa.fa-twitter(aria-hidden='true')

								a.btn.btn-facebook(
									href='/auth/social/twitter'
									data-toggle='tooltip'
									title='Login with Facebook'
								)
									i.fa.fa-facebook(aria-hidden='true')

					.form-group.m-b-0
						.col-sm-12.text-center
							p
								|Forgot your password? 
								router-link(to='/password' class='text-info m-l-5')
									b Reset here!
							p
								|Don't have an account? 
								router-link(to='/register' class='text-info m-l-5')
									b Sign Up
			guestFooter

</template>

<script>
import helper from '../../services/helper'
import GuestFooter from '../../layouts/guest-footer.vue'

export default {
	data () {
		return {
			loginForm: {
				email: '',
				password: ''
			}
		}
	},
	components: {
		GuestFooter
	},
	mounted () {},
	methods: {
		submit (e)
		{
			axios.post('/api/auth/login', this.loginForm)
			.then(response =>  {
				localStorage.setItem('auth_token', response.data.token)
				axios.defaults.headers.common['Authorization'] = 'Bearer ' + localStorage.getItem('auth_token')
				toastr['success'](response.data.message)
				this.$router.push('/home')
			})
			.catch(error => {
				toastr['error'](error.response.data.message)
			})
		}
	}
}
</script>

<style lang="scss" scoped>
.login-register {
	background-image: url('/images/background/background.jpg');
}
</style>
