事件名称	描述
abort	 在播放被终止时触发,例如, 当播放中的视频重新开始播放时会触发这个事件。
canplay	在媒体数据已经有足够的数据（至少播放数帧）可供播放时触发。这个事件对应CAN_PLAY的readyState。
canplaythrough	在媒体的readyState变为CAN_PLAY_THROUGH时触发，表明媒体可以在保持当前的下载速度的情况下不被中断地播放完毕。注意：手动设置currentTime会使得firefox触发一次canplaythrough事件，其他浏览器或许不会如此。
durationchange	元信息已载入或已改变，表明媒体的长度发生了改变。例如，在媒体已被加载足够的长度从而得知总长度时会触发这个事件。
emptied	媒体被清空（初始化）时触发。
ended	播放结束时触发。
error	在发生错误时触发。元素的error属性会包含更多信息。参阅Error handling获得详细信息。
loadeddata	媒体的第一帧已经加载完毕。
loadedmetadata	媒体的元数据已经加载完毕，现在所有的属性包含了它们应有的有效信息。
loadstart	在媒体开始加载时触发。
mozaudioavailable	当音频数据缓存并交给音频层处理时
pause	播放暂停时触发。
play	在媒体回放被暂停后再次开始时触发。即，在一次暂停事件后恢复媒体回放。
playing	在媒体开始播放时触发（不论是初次播放、在暂停后恢复、或是在结束后重新开始）。
progress	告知媒体相关部分的下载进度时周期性地触发。有关媒体当前已下载总计的信息可以在元素的buffered属性中获取到。
ratechange	在回放速率变化时触发。
seeked	在跳跃操作完成时触发。
seeking	在跳跃操作开始时触发。
stalled	在尝试获取媒体数据，但数据不可用时触发。
suspend	在媒体资源加载终止时触发，这可能是因为下载已完成或因为其他原因暂停。
timeupdate	元素的currentTime属性表示的时间已经改变。
volumechange	在音频音量改变时触发（既可以是volume属性改变，也可以是muted属性改变）.。
waiting	在一个待执行的操作（如回放）因等待另一个操作（如跳跃或下载）被延迟时触发。
使用下面的代码，你可以很容易的观察到这些事件:

var v = document.getElementsByTagName("video")[0];
v.addEventListener("seeked", function() { document.getElementsByTagName("video")[0].play(); }, true);
v.currentTime = 10.0;