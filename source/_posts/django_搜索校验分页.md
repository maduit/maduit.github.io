---
title: django_搜索校验分页
tag: django
categories: 2023
---

## 搜索

```python
##搜索
PrettyNum.objects.filter{mobile='13906135233',"id":123}
```

```python

data_dict={"mobile":"13906135233","id":123}

PrettyNum.objects.filter(**data_dict)
```

```
PrettyNum.objects.filter(id=12)  等于12
PrettyNum.objects.filter(id__gt=12) 大于12
PrettyNum.objects.filter(id__gte=12) 大于等于12
PrettyNum.objects.filter(id__lt=12)   小于12
PrettyNum.objects.filter(id__lte=12) 小于等于12

data_dict={"id__lte":12}
```

```
PrettyNum.objects.filter(mobile='233')  等于 PrettyNum.objects.filter(mobile__startswitch="139") 筛选出以139开头
PrettyNum.objects.filter(mobile__endswitch="233") 筛选出以233结尾
PrettyNum.objects.filter(mobile__contains="5233") 筛选出包含5233
```

```python
# use
data_dict={"mobile__contains":"233"}
PrettyNum.objects.filter(**data_dict)
```

## 校验

```
from django.core.exceptions import ValidationError
# 验证方法一
mobile=forms.CharField(
    label="手机号",
    disabled=True,
    validators=[RegexValidator(r'^1[3-9]\d{9}$','手机号格式错误')],
    )
```

```
from django.core.validators import RegexValidator
# 验证方法二
    def clean_mobile(self):
        txt_mobile=self.cleaned_data["mobile"]
        exists=PrettyNum.objects.filter(mobile=txt_mobile).exists()

        if len(txt_mobile)!=11:
            #验证不通过
            raise ValidationError("格式错误")
        if exists:
            #验证不通过
            raise ValidationError("手机号已存在")
        return txt_mobile
```

```
class PrettyForm(forms.ModelForm):
    # 验证方法一
    # mobile=forms.CharField(
    #     label="手机号",
    #     validators=[RegexValidator(r'^1[3-9]\d{9}$','手机号格式错误')]
    # )
    class Meta:
        model=PrettyNum
        fields=['mobile','price','level','status']
        # fields="__all__"
        # exclude=['level']
        widgets={
            "mobile":forms.TextInput(attrs={"class":"form-control"}),
            "price":forms.TextInput(attrs={"class":"form-control"}),
            "level":forms.TextInput(attrs={"class":"form-control"}),
            "status":forms.TextInput(attrs={"class":"form-control"}),
        }

# 验证方法二
    def clean_mobile(self):
        txt_mobile=self.cleaned_data["mobile"]
        exists=PrettyNum.objects.filter(mobile=txt_mobile).exists()

        if len(txt_mobile)!=11:
            #验证不通过
            raise ValidationError("格式错误")
        if exists:
            #验证不通过
            raise ValidationError("手机号已存在")
        return txt_mobile
```

```python
    data_dict={}
    value=request.GET.get('q','')
    if value:
        data_dict["mobile__contains"] = value   qurylist=PrettyNum.objects.filter(**data_dict).order_by("-level")
```

## 分页

```py
qurylist=PrettyNum.objects.all()#所有的
```

```python
qurylist=PrettyNum.objects.filter(id=4)[0:10]#id为4的前十页
```

```python
qurylist=PrettyNum.objects.all()[0:10]#第一页
```

```
qurylist=PrettyNum.objects.all()[10:20]#第二页
```

```
qurylist=PrettyNum.objects.all()[20:30]#第三页
```

```
    page=int(request.GET.get('page',1))
    start=(page-1)*10
    end=page*10
    qurylist=PrettyNum.objects.filter(**data_dict).order_by("-level")[start:end]
    return render(request,'pretty_list.html',{'qurylist':qurylist,"search_data":value})
```

```
  <nav aria-label="Page navigation">
    <ul class="pagination">
      <li>
        <a href="#" aria-label="Previous">
          <span aria-hidden="true">«</span>
        </a>
      </li>
      <li><a href="/pretty/list/?page=1">1</a></li>
      <li><a href="?page=2">2</a></li>
      <li><a href="?page=3">3</a></li>
      <li><a href="?page=4">4</a></li>
      <li><a href="?page=5">5</a></li>
      <li>
        <a href="#" aria-label="Next">
          <span aria-hidden="true">»</span>
        </a>
      </li>
    </ul>
  </nav>
```

```
#分页
    # qurylist=PrettyNum.objects.all()
    # qurylist=PrettyNum.objects.filter(id=4)[0:10]
    # qurylist=PrettyNum.objects.all()[0:10]
    # qurylist=PrettyNum.objects.all()[10:20]
    # qurylist=PrettyNum.objects.all()[20:30]
    page=int(request.GET.get('page',1))
    pageSize=10
    start=(page-1)*pageSize
    end=page*pageSize

    qurylist=PrettyNum.objects.filter(**data_dict).order_by("-level")[start:end]
   #总数据条数
    total_count=PrettyNum.objects.filter(**data_dict).order_by("-level").count()
   #总页码
    total_page_count,div=divmod(total_count,pageSize)
    if div:
        total_page_count+=1

    # 计算出，显示当前页的前5页、后5页
    plus=5
    if total_page_count<=2*plus+1:
        #数据库中的数据比较少，都没有达到11页
        start_page=1
        end_page=total_page_count
    else:
        #数据库中的数据比较多 >11页

        #当前页<5时(极小值)
        if page<=plus:
            start_page=1
            end_page=2*plus+1
        else:
            #当前页>5
            #当前页+5>总页面
            if (page+plus)>total_page_count:
                start_page=total_page_count-2*plus
                end_page=total_page_count
            else:
                start_page=page-plus
                end_page=page+plus
    #页码
    page_str_list=[]
    #首页
    page_str_list.append('<li><a href="?page={}">首页</a></li>'.format(1))
    #上一页
    if page>1:
        prev='<li><a href="?page={}">上一页</a></li>'.format(page-1)
    else:
        prev='<li><a href="?page={}">上一页</a></li>'.format(1)
    page_str_list.append(prev)
    
    for i in range(start_page,end_page+1):
        if i==page:
            ele='<li class="active"><a href="?page={}">{}</a></li>'.format(i,i)
        else:
            ele='<li><a href="?page={}">{}</a></li>'.format(i,i)
        page_str_list.append(ele)
    #下一页
    if page<total_page_count:
        prev='<li><a href="?page={}">下一页</a></li>'.format(page+1)
    else:
        prev='<li><a href="?page={}">下一页</a></li>'.format(total_page_count)
    page_str_list.append(prev)
    #尾页
    page_str_list.append('<li><a href="?page={}">尾页</a></li>'.format(total_page_count))
    
    page_string =mark_safe("".join(page_str_list))
    return render(request,'pretty_list.html',{'qurylist':qurylist,"search_data":value,"page_string":page_string})
```

```
def pretty_list(request):
##搜索
# --------------------
# 1.PrettyNum.objects.filter{mobile='13906135233',"id":123}
# 2.data_dict={"mobile":"13906135233","id":123}
#   PrettyNum.objects.filter(**data_dict)
# --------------------------------
# PrettyNum.objects.filter(id=12)  等于12
# PrettyNum.objects.filter(id__gt=12) 大于12
# PrettyNum.objects.filter(id__gte=12) 大于等于12
# PrettyNum.objects.filter(id__lt=12)   小于12
# PrettyNum.objects.filter(id__lte=12) 小于等于12

# data_dict={"id__lte":12}
# # -----------------------------------
# PrettyNum.objects.filter(mobile='233')  等于
# PrettyNum.objects.filter(mobile__startswitch="139") 筛选出以139开头
# PrettyNum.objects.filter(mobile__endswitch="233") 筛选出以233结尾
# PrettyNum.objects.filter(mobile__contains="5233") 筛选出包含5233
# use
# data_dict={"mobile__contains":"233"}
# PrettyNum.objects.filter(**data_dict)
# ----------------------------------------
# test
    # for i in range(300):
    #     PrettyNum.objects.create(mobile="13906135899",price=10,level=1,status=1)

    data_dict={}
    value=request.GET.get('q','')
    if value:
        data_dict["mobile__contains"] = value
    # res=PrettyNum.objects.filter(**data_dict)
    # print(res)

    # if request.method=='GET':
    # ****qurylist=PrettyNum.objects.filter(**data_dict).order_by("-level")[start:end]
    #select * from 表 order by level desc
    # qurylist=PrettyNum.objects.all().order_by("-level")
#分页
    # qurylist=PrettyNum.objects.all()
    # qurylist=PrettyNum.objects.filter(id=4)[0:10]
    # qurylist=PrettyNum.objects.all()[0:10]
    # qurylist=PrettyNum.objects.all()[10:20]
    # qurylist=PrettyNum.objects.all()[20:30]
    page=int(request.GET.get('page',1))
    pageSize=10
    start=(page-1)*pageSize
    end=page*pageSize

    qurylist=PrettyNum.objects.filter(**data_dict).order_by("-level")[start:end]
   #总数据条数
    total_count=PrettyNum.objects.filter(**data_dict).order_by("-level").count()
   #总页码
    total_page_count,div=divmod(total_count,pageSize)
    if div:
        total_page_count+=1

    # 计算出，显示当前页的前5页、后5页
    plus=5
    if total_page_count<=2*plus+1:
        #数据库中的数据比较少，都没有达到11页
        start_page=1
        end_page=total_page_count
    else:
        #数据库中的数据比较多 >11页

        #当前页<5时(极小值)
        if page<=plus:
            start_page=1
            end_page=2*plus+1
        else:
            #当前页>5
            #当前页+5>总页面
            if (page+plus)>total_page_count:
                start_page=total_page_count-2*plus
                end_page=total_page_count
            else:
                start_page=page-plus
                end_page=page+plus
    #页码
    page_str_list=[]
    #首页
    page_str_list.append('<li><a href="?page={}">首页</a></li>'.format(1))
    #上一页
    if page>1:
        prev='<li><a href="?page={}">上一页</a></li>'.format(page-1)
    else:
        prev='<li><a href="?page={}">上一页</a></li>'.format(1)
    page_str_list.append(prev)
    
    for i in range(start_page,end_page+1):
        if i==page:
            ele='<li class="active"><a href="?page={}">{}</a></li>'.format(i,i)
        else:
            ele='<li><a href="?page={}">{}</a></li>'.format(i,i)
        page_str_list.append(ele)
    #下一页
    if page<total_page_count:
        prev='<li><a href="?page={}">下一页</a></li>'.format(page+1)
    else:
        prev='<li><a href="?page={}">下一页</a></li>'.format(total_page_count)
    page_str_list.append(prev)
    #尾页
    page_str_list.append('<li><a href="?page={}">尾页</a></li>'.format(total_page_count))
    search_string=""""
  <form class="navbar-form navbar-left" method="GET">
    <div class="form-group">
      <input type="text" class="form-control" placeholder="Search" name="page">
    </div>
    <button type="submit" class="btn btn-default">Submit</button>
  </form>
    """
    page_str_list.append(search_string)
    page_string =mark_safe("".join(page_str_list))
    return render(request,'pretty_list.html',{'qurylist':qurylist,"search_data":value,"page_string":page_string})
```

```
{% extends 'layout.html' %}

{% block content %}


<div style="float: right;">

  <form class="navbar-form navbar-left" method="GET">
  <div class="form-group">
    <input type="text" class="form-control" placeholder="Search" name="q" value="{{search_data}}">
  </div>
  <button type="submit" class="btn btn-default">Search</button>
</form>

</div>
<div>            
  <a href="/pretty/add/" ><button type="button" class="btn btn-success dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">新建</button></a>    
  <!-- <a href="/usr/addform"><button type="button" class="btn btn-success dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">用form新建</button></a> -->
</div>

<div class="panel panel-default "> </div>

<div class="bs-example" data-example-id="table-within-panel">
  <div class="panel panel-default">
    <!-- Default panel contents -->
    <div class="panel-heading">靓号列表</div>
    <!-- <div class="panel-body">
    </div> -->

    <!-- Table -->
    <table class="table">
      <thead>
        <tr>
          <th>id</th>
          <th>手机号</th>
          <th>价格</th>
          <th>级别</th>
          <th>状态</th>
        </tr>
      </thead>

      <tbody>
          {% for mp in qurylist %}
        <tr>
          <th scope="row">{{mp.id}}</th>
          <td>{{mp.mobile}}</td>
          <td>{{mp.price}}</td>

          <!-- <td>{{mp.create_time}}</td> -->
          <!-- <td>{{mp.create_time|date:"Y-m-d H:i:s"}}</td> -->
          <td>{{mp.get_level_display}}</td>
          <td>{{mp.get_status_display}}</td>

          <td>
            <a href="/pretty/{{mp.id}}/edit/"><button type="button" class="btn btn-warning dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">编辑</button></a>  
           <!-- <a href="/dep/delete/?memid={{mp.id}}"><button type="button" class="btn btn-danger dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false" >删除</button></a> -->
           <!-- <a href="/dep/delete/?memid={{mp.id}}"><button type="button" class="btn btn-danger dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false" >删除</button></a> -->
           <a href="/pretty/{{mp.id}}/delete/"><button type="button" class="btn btn-danger dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false" >删除</button></a>
          </td>
        </tr>
      {% endfor %}
   
      </tbody>
    </table>
  </div>
  <nav aria-label="Page navigation">
    <ul class="pagination">
      {{page_string}}
    </ul>
  </nav>
  <!-- <form class="navbar-form navbar-left" method="GET">
    <div class="form-group">
      <input type="text" class="form-control" placeholder="Search" name="page">
    </div>
    <button type="submit" class="btn btn-default">Submit</button>
  </form> -->
{% endblock %}
```

## BootStrap样式父类

```
from django.shortcuts import render,HttpResponse,redirect
from .models import Department,Employee,PrettyNum
from django import forms
from django.core.exceptions import ValidationError
from django.core.validators import RegexValidator
from django.utils.safestring import mark_safe
from app01.utils.pagination import Pagination
import copy
# Create your views here.
def department_list(request):
    member_list=Department.objects.all()
    return  render(request,'dep.html',{'member':member_list})

def add_dep(request):
    if request.method=='GET':
        return render(request,'adddep.html')
    elif request.method=='POST':
        title=request.POST.get('aatitle')
        Department.objects.create(tittle=title)
        return redirect("/dep/list/")

def delete_dep(request):
    memid=request.GET.get('memid')
    Department.objects.filter(id=memid).delete()
    return redirect("/dep/list/")
def edit_dep(request,memid):
    if request.method=='GET':
        nnmame=Department.objects.filter(id=memid).first()
        return render(request,'editdpt.html',{'nnmame':nnmame.tittle})
    aatitle=request.POST.get('aatitle')
    Department.objects.filter(id=memid).update(tittle=aatitle)
    return redirect("/dep/list/")

def test(request):
    return render(request,'tt.html')
def usr_list(request):
        # for i in range(300):
        #     Employee.objects.create(name="maomao",password=123,age=50,create_time='2023-10-5',depart_id=5)
    
        qurylist=Employee.objects.all()

        page_object=Pagination(request,qurylist)

        context={
            "qurylist":page_object.page_qurylist,
            "page_string":page_object.html(),
        }
        return render(request,'usr.html',context)
# class MyForm(forms.Form):
    # usr=forms.CharField(widget=forms.EmailInput)
    # pwd=forms.CharField(widget=forms.PasswordInput)
    # age=forms.ImageField(widget=forms.NumberInput)
# class MyForm(forms.Form):
#     class Meta:
#         model=Employee
#         fields=['name','password','age']
def add_usr(request):
    
    context={
        'gender_choice':Employee.gender_choices,
        'depart_list':Department.objects.all()

    }
    # form=MyForm()
    
    return render(request,'add_usr.html',context)
    # return render(request,'add_usr_fro.html',{'form':form})


class MyForm(forms.ModelForm):
    # name=forms.CharField(min_length=3,label="用户名")
    # usr=forms.CharField(widget=forms.EmailInput)
    # pwd=forms.CharField(widget=forms.PasswordInput)
    # age=forms.ImageField(widget=forms.NumberInput)
    class Meta:
        model=Employee
        fields=['name','password','age','account','create_time','depart','gender']
        widgets={
            "name":forms.TextInput(attrs={"class":"form-control"}),
            "age":forms.TextInput(attrs={"class":"form-control"}),
            "create-time":forms.TextInput(attrs={"type":"data"})
        }
        # def __init__(self,*args, **kwargs):
        #    super().__init__(*args,**kwargs)
        #
        #    for name,field in self.fields.items():
        #        field.Widget.attrs={"class":"form-control","placeholder":field.label}

def add_usr_form(request):
    if request.method == "GET":
      form=MyForm()
      return render(request,'add_usr_fro.html',{'form':form})
    
    # post 校验
    form=MyForm(data=request.POST)
    if form.is_valid():
        form.save()
        return redirect('/usr/list/')
    else:
        return render(request,'add_usr_fro.html',{"form":form})
def usr_edit(request,nid):
    #获取默认值
    row_object=Employee.objects.filter(id=nid).first()
    if request.method =='GET':
        form =MyForm(instance=row_object)
        return render(request,'usr_edit.html',{'form':form})
    #post
    form=MyForm(data=request.POST,instance=row_object)
    if form.is_valid():
        form.save()
        return redirect('/usr/list/')
    return render(request,'usr_edit.html',{'form':form})
def usr_delete(request,nid):
    Employee.objects.filter(id=nid).delete()
    return redirect('/usr/list/')
class PrettyForm(forms.ModelForm):
    # 验证方法一
    # mobile=forms.CharField(
    #     label="手机号",
    #     validators=[RegexValidator(r'^1[3-9]\d{9}$','手机号格式错误')]
    # )
    class Meta:
        model=PrettyNum
        fields=['mobile','price','level','status']
        # fields="__all__"
        # exclude=['level']
        widgets={
            "mobile":forms.TextInput(attrs={"class":"form-control"}),
            "price":forms.TextInput(attrs={"class":"form-control"}),
            "level":forms.TextInput(attrs={"class":"form-control"}),
            "status":forms.TextInput(attrs={"class":"form-control"}),
        }

# 验证方法二
    def clean_mobile(self):
        txt_mobile=self.cleaned_data["mobile"]
        exists=PrettyNum.objects.filter(mobile=txt_mobile).exists()

        if len(txt_mobile)!=11:
            #验证不通过
            raise ValidationError("格式错误")
        if exists:
            #验证不通过
            raise ValidationError("手机号已存在")
        return txt_mobile
def pretty_list(request):
##搜索
# --------------------
# 1.PrettyNum.objects.filter{mobile='13906135233',"id":123}
# 2.data_dict={"mobile":"13906135233","id":123}
#   PrettyNum.objects.filter(**data_dict)
# --------------------------------
# PrettyNum.objects.filter(id=12)  等于12
# PrettyNum.objects.filter(id__gt=12) 大于12
# PrettyNum.objects.filter(id__gte=12) 大于等于12
# PrettyNum.objects.filter(id__lt=12)   小于12
# PrettyNum.objects.filter(id__lte=12) 小于等于12

# data_dict={"id__lte":12}
# # -----------------------------------
# PrettyNum.objects.filter(mobile='233')  等于
# PrettyNum.objects.filter(mobile__startswitch="139") 筛选出以139开头
# PrettyNum.objects.filter(mobile__endswitch="233") 筛选出以233结尾
# PrettyNum.objects.filter(mobile__contains="5233") 筛选出包含5233
# use
# data_dict={"mobile__contains":"233"}
# PrettyNum.objects.filter(**data_dict)
# ----------------------------------------
# test
    # for i in range(300):
    #     PrettyNum.objects.create(mobile="13906135899",price=10,level=1,status=1)
   
   
    # print(request.GET)
    # request.GET.setlist('xx',11)
    # print(request.GET.urlencode())

    #q=123&page=2

    # get_object=copy.deepcopy(request.GET)
    # get_object._mutable=True
    # get_object.setlist('page',[11])
    # print(get_object.urlencode())
    qury_dict=copy.deepcopy(request.GET)
    qury_dict._mutable=True
    qury_dict.setlist('page',[11])
    print(qury_dict.urlencode())



    data_dict={}
    value=request.GET.get('q','')
    if value:
        data_dict["mobile__contains"] = value


    qurylist=PrettyNum.objects.filter(**data_dict).order_by("-level")
    page_object= Pagination(request,qurylist)
    page_qurylist=page_object.page_qurylist
    page_string=page_object.html()
    context={'qurylist':page_qurylist,
             "search_data":value,
             "page_string":page_string}
    # res=PrettyNum.objects.filter(**data_dict)
    # print(res)

    # if request.method=='GET':
    # ****qurylist=PrettyNum.objects.filter(**data_dict).order_by("-level")[start:end]
    #select * from 表 order by level desc
    # qurylist=PrettyNum.objects.all().order_by("-level")
#分页
    # qurylist=PrettyNum.objects.all()
    # qurylist=PrettyNum.objects.filter(id=4)[0:10]
    # qurylist=PrettyNum.objects.all()[0:10]
    # qurylist=PrettyNum.objects.all()[10:20]
    # qurylist=PrettyNum.objects.all()[20:30]
    # page=int(request.GET.get('page',1))
    # pageSize=10
    # start=(page-1)*pageSize
    # end=page*pageSize

    # qurylist=PrettyNum.objects.filter(**data_dict).order_by("-level")[page_object.start:page_object.end]
   #总数据条数
#     total_count=PrettyNum.objects.filter(**data_dict).order_by("-level").count()
#    #总页码
#     total_page_count,div=divmod(total_count,pageSize)
#     if div:
#         total_page_count+=1

    # 计算出，显示当前页的前5页、后5页
#     plus=5
#     if total_page_count<=2*plus+1:
#         #数据库中的数据比较少，都没有达到11页
#         start_page=1
#         end_page=total_page_count
#     else:
#         #数据库中的数据比较多 >11页

#         #当前页<5时(极小值)
#         if page<=plus:
#             start_page=1
#             end_page=2*plus+1
#         else:
#             #当前页>5
#             #当前页+5>总页面
#             if (page+plus)>total_page_count:
#                 start_page=total_page_count-2*plus
#                 end_page=total_page_count
#             else:
#                 start_page=page-plus
#                 end_page=page+plus
#     #页码
#     page_str_list=[]
#     #首页
#     page_str_list.append('<li><a href="?page={}">首页</a></li>'.format(1))
#     #上一页
#     if page>1:
#         prev='<li><a href="?page={}">上一页</a></li>'.format(page-1)
#     else:
#         prev='<li><a href="?page={}">上一页</a></li>'.format(1)
#     page_str_list.append(prev)
    
#     for i in range(start_page,end_page+1):
#         if i==page:
#             ele='<li class="active"><a href="?page={}">{}</a></li>'.format(i,i)
#         else:
#             ele='<li><a href="?page={}">{}</a></li>'.format(i,i)
#         page_str_list.append(ele)
#     #下一页
#     if page<total_page_count:
#         prev='<li><a href="?page={}">下一页</a></li>'.format(page+1)
#     else:
#         prev='<li><a href="?page={}">下一页</a></li>'.format(total_page_count)
#     page_str_list.append(prev)
#     #尾页
#     page_str_list.append('<li><a href="?page={}">尾页</a></li>'.format(total_page_count))
#     search_string=""""
#   <form class="navbar-form navbar-left" method="GET">
#     <div class="form-group">
#       <input type="text" class="form-control" placeholder="Search" name="page">
#     </div>
#     <button type="submit" class="btn btn-default">Submit</button>
#   </form>
#     """
#     page_str_list.append(search_string)
#     page_string =mark_safe("".join(page_str_list))
    return render(request,'pretty_list.html',context)
def pretty_add(request):
    if request.method=='GET':
        form=PrettyForm()
        return render(request,'pretty_add.html',{'form':form})
    #post校验
    form=PrettyForm(data=request.POST)
    if form.is_valid():
        form.save()
        return redirect('/pretty/list/')
    else:
        return render(request,'pretty_add.html',{"form":form})
class PrettyEditModelForm(forms.ModelForm):
        # 验证方法一
    mobile=forms.CharField(
  
        label="手机号",
        # disabled=True,
        validators=[RegexValidator(r'^1[3-9]\d{9}$','手机号格式错误')],
    ) 
    ## mobile=forms.CharField(disabled=True,label="手机号")

    class Meta:
        model=PrettyNum
        fields=['mobile','price','level','status']
    def clean_mobile(self):
    #     print(self.instance.pk)
        
        txt_mobile=self.cleaned_data["mobile"]
        exists=PrettyNum.objects.exclude(id=self.instance.pk).filter(mobile=txt_mobile).exists()
        # if len(txt_mobile)!=11:
        #     # 验证不通过
        #     raise ValidationError("格式错误")
        if exists:
            #验证不通过
            raise ValidationError("手机号已存在")
        return txt_mobile
       
def pertty_edit(request,nid):
    row_object=PrettyNum.objects.filter(id=nid).first()
    if request.method=='GET':
        form=PrettyEditModelForm(instance=row_object)
        return render(request,'pretty_edit.html',{'form':form})
    #post
    form=PrettyEditModelForm(data=request.POST,instance=row_object)
    if form.is_valid():
        form.save()
        return redirect('/pretty/list/')
    else:
        return render(request,'pretty_edit.html',{'form':form})
def pertty_delete(request,nid):
    PrettyNum.objects.filter(id=nid).delete()
    return redirect('/pretty/list/')
```

