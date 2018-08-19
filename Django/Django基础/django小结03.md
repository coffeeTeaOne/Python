

web页面Django的增删改查, 主要是代码示例



#### Ajax 删除空的部门

```html

<tbody id="dept">
	{% for dept in dept_list %}
		<tr>
          <td><a id="{{ dept.no }}" href="javascript:void(0)"><button type="button" class="btn btn-danger btn-xs">删除</button></a>
            </td>
		</tr>
	{% endfor %}
</tbody>

<script>
$(function() {
    $('#dept a[id]').on('click', function(evt) {
        var a = $(evt.target).parent();
            if (confirm('确定??')) {
           $.getJSON('/hrs/deldept/' + a.attr('id'), function(json) {
                   if (json.code == 200) {
                       a.parent().parent().remove()
                        }
                  });
             }
          });
	})
</script>
```



```python
# urls
path('deldept/<int:no>', views.del_dept, name='ddel')
# views
def del_dept(request, no='0'):
    try:
        # no = request.GET['no']
        Dept.objects.get(pk=no).delete()
        ctx = {'code': 200}    #  Ajax
    except (ObjectDoesNotExist, ValueError, MultiValueDictKeyError):
        ctx = {'code':404}
    return HttpResponse(dumps(ctx),
                        content_type='application/json; '
                                     'charset=utf-8') # 返回类型
    # return HttpResponseRedirect(reverse('depts'))
    # return redirect(reverse('depts'))  # reverse  重定向

```



- 通过跳转页面传参数,删除

```html
<td><button type="button" class="btn btn-danger btn-xs"><a href="{% url 'ddel' dept.no %}">删除</a></button></td>
```





#### 添加违章记录, 表单提交

```html
<!--add.html-->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>添加</title>
</head>
<body>
    <h2>添加违章记录</h2>
    <hr>
    <form action="/add" method="post">
        {% csrf_token %}
        <table>
            {{ f.as_table }}
        </table>
        <input type="submit" value="添加">
    </form>
</body>
</html>

```



```python
# url
path('add', views.add)

# views
class CarRecordForm(forms.Form):

    carno = forms.CharField(max_length=7 ,label='车牌号', error_messages={})
    case = forms.CharField(max_length=50, label='违章原因')
    punish = forms.CharField(max_length=50, required=False, label='处罚方式') #required 属性False可以不填


def add(request):

    if request.method == 'GET':
        errors = []
        f = CarRecordForm()
    else:
        f = CarRecordForm(request.POST)
        if f.is_valid():
            a = f.cleaned_data # 获得输入的纯数据, 字典样式
            Car(**a).save()
            f = CarRecordForm()
        else:
            errors = f.errors.values()  # 所有的错误信息
    return render(request, 'add.html', context={'f': f, 'errors': errors})
```





#### 跳转 ,查找该部门下属的员工

```html
<td>
	<a href="{% url 'empsindept' dept.no %}">{{ dept.name }}</a>
</td>
```



```python
# url
path('depts/emps/<int:no>', views.emps, name='empsindept')

# views
def emps(request, no='0'):
    # dno = request.GET['no']
    dno = no
    emp_list = list(Emp.objects.filter(dept__no=dno).select_related('dept'))  # 联查, 关联对象字段名, models的class Dept
    ctx = {
        'emp_list': emp_list,
        'dept_name': emp_list[0].dept.name
    } if len(emp_list) > 0 else {}
    return render(request, 'emp.html', context=ctx)
```



#### Ajax-查找, 查找违章记录

```html
<script>
        $(function() {
            $('#search').on('submit', function (evt)  {
                evt.preventDefault(); //阻止默认的提交行为
                var carno = $('#carno').val();
                var token = $('#search input[type=hidden]').val();
                $.ajax({
                    url: '/search2',
                    type: 'post',
                    data: {
                        'carno': carno,
                        'csrfmiddlewaretoken': token
                    },
                    dataType: 'json',
                    success: function(json) {
                        $('#result tbody').children().remove();
                        alert(json.length);
                        for (var i = 0; i < json.length; i += 1) {
                            var tr = $('<tr>').append($('<td>')).txt(json);
                            $('#result tbody').append(tr);
                        }
                    }
                });
            });
        });
    </script>
```

```python
class CarRecordEncoder(DjangoJSONEncoder):  # DjangoJSONEncoder
    # pass
    def default(self, o):

        del o.__dict__['_state']
        print('===========>>>>>>>>>', o.__dict__)
        # o.__dict__['cardate'] = o.happen_date
        return o.__dict__


def ajax_search(request):
    if request.method == "GET":
        return render(request, 'ajax_search.html')
    else:
        carno = request.POST['carno']
        record_list = list(Car.objects.filter(carno__icontains=carno))
        return JsonResponse(record_list,encoder=CarRecordEncoder ,safe=False)

```

