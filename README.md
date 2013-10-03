loaddirective
=============

loaddirective directive to load directives!

Imagine you have a view, and you want it to load a directive, which name is stored in a scope variable named "directiveNameInScope".
Naively, you would want it to work like 

```html
<div {{directiveNameInScope}}></div>
```

But that doesn't work. So I created a directive to do it for you. It works like

```html
<div loaddirective="directiveNameInScope"></div>
```

where the loaddirective directive looks like

```javascript
Application.Directives.directive('loaddirective', function($compile) {
	return {
		restrict: 'A',
		scope: { loaddirective : "=loaddirective" },
		link: function($scope, $element, $attr) {
			var value = $scope.loaddirective;
			if (value) {
				// Load the directive and make it reactive
				$element.html("<div "+value+"></div>");
				$compile($element.contents())($scope);
			}
		},
		replace: true
	};
});
```

Works!
