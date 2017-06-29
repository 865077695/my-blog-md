---
title: 关于checked
date: 2016-12-07 10:21:29
tags: Skill
---

### checkbox 属性checked="checked"通过js设置之后，为什么没有勾选。
#### 通过attr('checked','checked')来设置checkbox时，重复点击，虽然checked属性设置正确，但是**checkbox没有被勾选**，如下代码(全选功能):
```js
$('#ckAll').click(function(){
    if($('#ckAll ').attr('checked') == 'checked'){
        $('#ckAll').removeAttr('checked');
    }else{
        $('#ckAll').attr('checked','checked');
    }
    if($('#ckAll').attr('checked') == 'checked'){
        $('.tab-list .ckbox').each(function(i,n){
            $(n).attr('checked','checked');
        });
    }else{
        $('.tab-list .ckbox').each(function(i,n){
            $(n).removeAttr('checked');
        });
    }
}); 
```
#### 换成prop('checked',true),当ckAll被选中时，所有列表checkbox都会被选中
```js
$('#ckAll').click(function(){
    if($('#ckAll').prop('checked')){
        $('.tab-list .ckbox').each(function(i,n){
            $(n).prop('checked',true);
        });
    }else{
        $('.tab-list .ckbox').each(function(i,n){
            $(n).prop('checked',false);
        });
    }
});
```
