完美使用restframework+django_filters实现api接口过程



环境：

`pip install restframework=3.4.6`

`pip install django_filter`



project->settings.py

```python

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'test',
    'rest_framework', # 注册app
]

# 修改配置  rest_framework
REST_FRAMEWORK = {
    # 将 restframework 默认的用户认证取消
    'DEFAULT_AUTHENTICATION_CLASS': (),
    # 筛选过滤数据
    'DEFAULT_FILTER_BACKENDS':(
        'rest_framework.filters.DjangoFilterBackend',
        'rest_framework.filters.SearchFilter',
    ),
}
```

app->urls.py

```python


from django.conf.urls import url
from rest_framework.routers import SimpleRouter

from market import views


router = SimpleRouter()
router.register(r'^goods', views.GoodsApi)

urlpatterns = [
    url(r'^index/', views.index, name='index'),
]
urlpatterns += router.urls

```

app->views.py

```python
class GoodsApi(mixins.ListModelMixin,  # get
               mixins.CreateModelMixin,  # post
               mixins.RetrieveModelMixin,  # 单个查询
               mixins.UpdateModelMixin,  # 修改 put 全部  patch 部分
               mixins.DestroyModelMixin,  # 删除 delete
               viewsets.GenericViewSet):
    """商品 API """
    # 从数据库中获取的数据
    queryset = Goods.objects.filter(good_id__lt=200)
    # 数据序列化器, 将查询的数据进行处理,返回格式良好的数据
    serializer_class = GoodSerializer
    # 自定义的过滤器,对数据进行筛选
    filter_class = GoodsFilter
```

utils->serializers.py

```python

from rest_framework import serializers

from market.models import Goods


class GoodSerializer(serializers.ModelSerializer):
    """自定义商品序列化器"""
    class Meta:
        model = Goods

    # 对获取到的数据进行数据格式化
    def to_representation(self, instance):
        data = super().to_representation(instance)
        category_id= data.pop('category')
        data['brand_id'] = brand_id
        return data

```

utils->filters.py

```python

import django_filters
from rest_framework import filters

from market.models import Goods, GoodsBrand, GoodsCategory


class GoodsFilter(filters.FilterSet):
    """用于商品查询的过滤器"""
    class Meta:
        model = Goods
        # 用于查询的字段
        fields = '__all__'

    # 定义查询的方法 contains:模糊查询; gte:大于等于; lte:小于等于
    # url : /market/goods/?good_id_lte=160&  添加其他条件
    good_id = django_filters.CharFilter('good_id', lookup_expr='exact')
    good_name = django_filters.CharFilter('good_name', lookup_expr='icontains')
    good_category_id = django_filters.CharFilter('category_id', lookup_expr='exact')
    good_brand_id = django_filters.CharFilter('brand_id', lookup_expr='exact')
    good_amount_lte = django_filters.CharFilter("real_amount", lookup_expr='lte')
    good_amount_gte = django_filters.CharFilter("real_amount", lookup_expr='gte')

```

#### 接口访问的方法说明

### GET 请求

```python

GET  /market/goods/? + params
```

#### params

```python

good_id=<id>  # 获取 id 的商品
good_name=<name>  # 模糊查询 获取该名字的商品
good_category_id=<id>  # 商品分类id
good_brand_id=<id>  # 商品品牌 id
good_amount_lte=<price>  # 获取商品价格小于 price 的商品
good_amount_gte=<price>  # 获取商品价格大于 price 的商品

# 可以多个条件一起查询
good_amount_lte=<price>&good_amount_gte=<price>
```






