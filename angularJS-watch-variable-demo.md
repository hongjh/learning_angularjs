在使用AngulaJS编写应用时，我们经常需要做的一件事情就是对模型中的变量进行监视，并对其发生的变化做出相应的回应。如：购物车小计。

AngularJS为我们提供了一个非常方便的$watch方法，它可以帮助我们在每个scope中监视其中的变量。下面是一个非常简单的例子：


```

$scope.name = 'zhangsan';  
  
$scope.count = 0;  
  
$scope.cart = [  
    {id:1,name:'iphone5s',quantity:3,price:4300},  
    {id:2,name:'iphone5c',quantity:30,price:3300},  
    {id:3,name:'mac',quantity:3,price:14300},  
    {id:4,name:'ipad',quantity:100,price:2000}  
];  
  
// 监听一个model 当一个model每次改变时 都会触发第2个函数  
$scope.$watch('name', function(newValue, oldValue) {  
    // console.log(newValue+ '===' +oldValue);  
    ++$scope.count;  
  
    if ($scope.count > 3) {  
        $scope.name = '已将大于3次了';  
    }  
});  
  
$scope.$watch('cart', function(newValue, oldValue) {  
    // console.log(newValue);  
    angular.forEach(newValue, function(item, key) {  
        if(item.quantity < 1) {  
            var returnKey = confirm('是否从购物车内删除该产品');  
            if(returnKey) {  
                $scope.remove(item.id);  
            }else{  
                item.quantity = oldValue[key].quantity;  
            }  
            return ;  
        }  
    });  
}, true); ////检查被监控的对象的每个属性是否发生变化

```

上面的这段代码非常简单，它用$watch来对$scope中的name进行监视，并在它发生变化的时候将$rootScope中的count属性增加1。
因此，每当我们对name进行一次修改时，下面显示的change count数字就会增加1,当count>3时，改变name的值。

在AngularJS内部，每当我们对ng-model绑定的name属性进行一次修改，AngularJS内部的$digest就会运行一次，
并在运行结束之后检查我们使用$watch来监视的东西，如果和进行上一次$digest之前相比有了变化，则执行我们在其中绑定的处理函数。