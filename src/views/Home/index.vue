<template>
	<el-container class="container">
		<el-header class="header">
			<div class="logo">
				<img src="../../assets/logo.png" />
				<span>电商后台管理系统</span>
			</div>

			<el-button @click="logout">退出</el-button>
		</el-header>
		<el-container>
			<el-aside
				class="aside"
				:width="isCollapse ?'64px' : '200px'"
			>
				<!-- <div class="changeMenuDiv">X</div> -->
				<div
					class="toggle_button"
					:class="isCollapse ? 'el-icon-s-unfold' : 'el-icon-s-fold'"
					@click="toggleMenu"
				></div>
				<el-menu
					class="menu"
					background-color="#333744"
					text-color="#fff"
					active-text-color="#409bff"
					unique-opened
					:collapse="isCollapse"
					:collapse-transition="false"
					:default-active="$route.path"
					router
				>
					<el-submenu
						v-for="item in menuList"
						:key="item.id"
						:index="item.id + ''"
					>
						<template slot="title">
							<i :class="menuIcon[item.id]"></i>
							<span>{{ item.authName }}</span>
						</template>
						<el-menu-item
							v-for="cItem of item.children"
							:key="cItem.id"
							:index="`/${cItem.path}`"
						>
							<template slot="title">
								<i class="el-icon-menu"></i>
								<span>{{ cItem.authName }}</span>
							</template>
						</el-menu-item>
					</el-submenu>
				</el-menu>
			</el-aside>
			<el-main class="main">
				<router-view></router-view>
			</el-main>
		</el-container>
	</el-container>
</template>

<script>
export default {
	created() {
		this.getMenuList()
	},
	data() {
		return {
			menuList: [],
			menuIcon: {
				// 125 103 101 102 145
				125: 'el-icon-user-solid',
				103: 'el-icon-open',
				101: 'el-icon-sell',
				102: 'el-icon-s-claim',
				145: 'el-icon-s-data'
			},
			isCollapse: false
			// menuIcon: {
			// 	// 125 103 101 102 145
			// 	125: 'iconfont icon-user',
			// 	103: 'iconfont icon-tijikongjian',
			// 	101: 'iconfont icon-shangpin',
			// 	102: 'iconfont icon-danju',
			// 	145: 'iconfont icon-baobiao'
			// }
		}
	},
	methods: {
		logout() {
			window.sessionStorage.removeItem('token')
			this.$router.push('/login')
		},
		async getMenuList() {
			const { data: res } = await this.$http.get('menus')
			if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
			this.menuList = res.data
		},
		toggleMenu() {
			this.isCollapse = !this.isCollapse
		}
	}
}
</script>

<style lang="less" scoped>
.container {
	height: 100%;
}
.header {
	background-color: #373d41;
	display: flex;
	justify-content: space-between;
	align-items: center;
	padding-left: 0;
	.logo {
		display: flex;
		align-items: center;
		height: 40px;
		img {
			width: 40px;
			height: 40px;
		}
		span {
			margin-left: 15px;
			color: #fff;
		}
	}
}
.aside {
	background-color: #333744;
	transition: all 0.5s;
	// position: relative;
	// overflow: visible;
	.menu {
		border: 0;
		.iconfont {
			margin-right: 10px;
		}
	}
	// .changeMenuDiv {
	// 	position: absolute;
	// 	width: 30px;
	// 	height: 30px;
	// 	right: -30px;
	// 	top: 0;
	// 	background-color: #333744;
	// 	color: #fff;
	// 	border-radius: 0 5px 5px 0;
	// 	text-align: center;
	// 	line-height: 30px;
	// 	z-index: 2;
	// 	cursor: pointer;
	// }
	.toggle_button {
		width: inherit;
		&::before {
			width: inherit;
			display: inline-block;
			background-color: #333744;
			color: #fff;
			text-align: center;
			height: 30px;
			font-size: 20px;
			line-height: 30px;
			cursor: pointer;
		}
	}
}
.main {
	background-color: #eaedf1;
}
</style>
