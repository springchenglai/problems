#1.js类型判断
##问题现象
typeof无法判断引用类型，如数组和对象使用typeof判断都返回"object"
##问题原理
无
##解决方案
用instanceof或constructor或toString.call()来判断，其中toString.call()为万能方案，任何情况下都适用
#2.js精度丢失
##问题现象
js中出现精度丢失，例如(1) 0.1 + 0.2 != 0.3 (2) 极大整数
##问题原理
1. 浮点数在转换为二进制表示的时候，可能会有无限循环的情况，无法精确求出二进制表示，因此只能0舍1入，导致计算出来的结果丢失精度
2. JS存在最大安全数，大于该数可能会丢失精度
##解决方案
把小数转换成整数（乘倍数）再参与运算，运算结果再缩小回原来倍数（除倍数），极大整数用字符串进行存储与传输
#3.click的300ms延迟响应
##问题现象
click的300ms延迟响应
##问题原理
由于用户可以进行双击缩放或者双击滚动的操作，当用户一次点击屏幕之后，浏览器并不能立刻判断用户是确实要打开这个链接，还是想要进行双击操作。因此，移动端浏览器就等待 300 毫秒，以判断用户是否再次点击了屏幕
##解决方案
使用FastClick
#4.type=file input
##问题现象
导入文件1成功，再次导入文件1页面无反应
##问题原理
file类型input的value值为选择的第一个文件的文件路径，如果两次选择同一文件，路径值相同，不会触发input的change事件
##解决方案
每次选择后清空input的value值
#5.ios滑动穿透
##问题现象
ios中，弹框出现时，底层页面依然能滑动
##问题原理
滑动穿透事件
##解决方案
1. 弹框出现时，手动让底层容器不可滑动，overflow-y: hidden
2. 阻止最外层容器的滚动行为
> $(selector).on('touchmove',function(e) {
   e.preventDefault()
})
#6.vuex状态保存
##问题现像
vue项目中，页面刷新之后，存在vuex的数据初始化了，导致页面状态不保存
##问题原理
vuex保存状态不是持久化保存的
##解决方案
配合h5的localStorage或者sessionStorage使用，区别是前者可持久化存储，后者的存储有效期是一个会话期间
#7.微信静默授权
##问题现象
在微信浏览器中进入支付页面，页面刷了一下，然后点击手机的返回按钮无法返回到上一页，总是停留在当前页
##问题原理
进入支付页面时，判断当前在微信环境的情况下，为了获取openid，会去请求微信内部鉴权地址进行微信静默授权，而授权后重定向到redirect_uri指定的地址会增加历史记录，redirect_uri指定一个中间页，这个中间页会把授权得到的code即openid增加到页面地址中然后location.replace到该页。所以，最后的历史记录栈为：下单前页面->无code下单页->有code下单页。点击返回的时候，返回到无code下单页，然后无code下单页又去请求授权，最后又跳到有code下单页，如此反复，陷入死循环。
##解决方案
授权成功后，在重定向的中间页中通过sessionStorage存储code，返回到无code下单页时判断code是否存在，如果存在则不再进行一系列授权操作。
##8.ios端软键盘弹出导致布局问题
##问题现象
ios端软键盘弹出时，页面会向上移，键盘收起后，页面没有恢复回来，导致页面底部留白
##解决方案
监听表单节点的blur事件，在失焦时：
1. 主动触发页面的滚动，因为页面滚动后布局就恢复正常了
2. 让页面滚回到视口区域，即document.activeElement.scrollIntoView(true)
