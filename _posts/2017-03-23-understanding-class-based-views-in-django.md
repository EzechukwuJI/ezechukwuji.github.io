---
layout: blogpost
title: understanding django class based view
navigation_weight: 3
comments: true
---

## UNDERSTANDING CLASS BASED VIEWS IN DJANGO
We often find that Django developers especially those new to the framework and even some advanced users prefer to use function based views (FBVs) for most projects. This can be attributed firstly to the simplicity of 
using FBVs and a lack of understanding of the great benefits of using Class Based Views.(CBV)
In this post I will highlight some of the major benefits and the cool features of CBV. This post will also attempt to simplify some of the most misunderstood concepts of CBV. We will look at cases where CBVs are preferrable to FBvs
and where the otherwise is the case.  

### Some Advantages of Using Class Based View
* Code reuseability - In CBV, a view class can be inherited by another view class and modified for a different use case. 
* DRY - Using CBVs help to reduce code duplication
* Code extendability - CBV can be extended to include more functionalities using Mixins
* Code structuring - In CBVs A class based view helps you respond to different http request with different class instance methods instead of conditional branching statements inside a single function based view.
    
Django class based view provides a class instance method `as_view()` which serves as an entry point for any generic CBV. Django URL resolver expects to send the request passed through it to a callable(a function). The `as_view()` class instance method in each generic view creates a hook for calling the class just like a method.
As such, the url configuration for calling a CBV follows the pattern below.

	urlpatterns = [
		url(r'^homepage/', ClassView.as_view(), name = "url_name") 
    ]

The `as_view()` method then calls the `dispatch()` method which in turn calls the class instance method defined for the applicable http request method if available or throws an  `HttpResponseNotAllowed` exception.

	CBV.as_view() ------> CBV.dispatch() --------------------
                                         |     |     |      | and so on.
                                         v     v     v      v
                                        get   post  head   put
					
Base class attributes can be overriden in child class as with the standard Python approach. Alternatively, the new attribute can be passed as an argument to the `as_view()` method of the CBV 

Example: In base class we have 

	class BaseClass(View):
	     greetings  =  "Good morning"
             def get(self, request):
                return HttpResponse(greetings)

    class ChildClass(View):
         greetings  =  "Good afternoon"
         def get(self, request):
            return HttpResponse(greetings)

This can also be done this in the url configuration  as shown below, 
	
    urlpatterns = [
		url(r'^homepage/', ChildClass.as_view(overriden_attrbute = "new value"), name = "url_name") ]

Basically, a django CBV inherits from the generic view class.
However, a CBV can inherit from any of the available generic views. 
NB: a CBV can only have one parent generic parent view, its functionality can also be improved through the use of Mixins. 

### Using Mixins

One of the advantages of using a Class based view its extendability, this quality is further enhanced through the use of Mixins.
Mixins are a form of multiple inheritances that allows the combination of behaviour from multiple parent classes.They are used to add additional functionality and behavior to classes. Mixins are meant to be inherited and not implemented directly. In CBVs Mixins are passed as leftmost argument while Django generic view classes are placed to the right. 
It is worth noting that Extending a class view with multiple Mixins can lead to some unexpected behaviour,make it harder to read a child class to know what it is doing and the harder it will be to know which method from which Mixin to overide. However, If used with caution, mixins are great tools for adding robust functionalities to a CBV. see example below.;

	class NewMixin(object):
		def do_whatever(self, *args, **kwargs):
			  #do whatever

The above mixin is used as below

	from django.views.generic import TemplateView
  
	class FreshViewClass(NewMixin, TemplateView):
		def do_something_awesome(self):
			  continue here
            
The methods defined in the mixin can be called directly in the `FreshViewClass` class. 

### Handling Forms with CBV

Unlike in function based views where http methods are evaluated in a conditional branching statements,Class Based Views handle each request type in a distinct class instance method named correspondingly to the request method.Example, the `get()` method handles GET request and the `post()` method handles POST requests.

      from django.http import HttpResponseRedirect
      from django.shortcuts import render
      from django.views.generic import View
      from .forms import MyForm

      class MyFormView(View):
          form_class = MyForm
          initial = {'key': 'value'}
          template_name = 'form_template.html'

          def get(self, request, *args, **kwargs):
              form = self.form_class(initial=self.initial)
              return render(request, self.template_name, {'form': form})

          def post(self, request, *args, **kwargs):
              form = self.form_class(request.POST)
              if form.is_valid():
                  # <process form cleaned data>
                  return HttpResponseRedirect('/success/')
              return render(request, self.template_name, {'form': form})
        
This class can easily be extended by customizing the class attributes such as template and the form_class in another class that inherits it.

### Using Decorators In Class Based Views

Decorators can be passed to CBV using any of these 3 approaches.
I. In the url configurations 
    This approach is used to apply decorators on a per-instance bases.
    
          from django.contrib.auth.decorators import login_required, permission_required
          from django.views.generic import TemplateView
          from .views import MyViewClass

          urlpatterns = [       url(r'^about/', login_required(TemplateView.as_view(template_name="secret.html"))),
          url(r'^vote/', permission_required('polls.can_vote')(MyViewClass.as_view())),]   
    
II. Decorating the class `dispatch()` method
The `as_view()` which is passed into the url of a CBV calls the `dispatch()` method of the view class which, in turn, calls the corresponding http request method 	  invoked. Essentially, the `dispatch()` method can be seen as the main entry_point of any CBV and so it makes sense to put our decorators here. To do this, the `@method_decorator()` wrapper is used to transform a view decorator into a method 	decorator. This is because a class method isn't exactly the same as a function, 	as such, function decorators can't be used directly on 

        class methods. 
            @method_decorator(login_required)
            def dispatch(self, *args, **kwargs):
                return super(ProtectedView, self).dispatch(*args, **kwargs)

Multiple common decorators can be passed in a list or tuple and called at once inside the `@method_decorator()` wrapper.

	    decorators = ['permission 1','permission2'...]
	    @method_decorator(decorators)
    	def dispatch(self, *args, **kwargs):
        	return super(ProtectedView, self).dispatch(*args, **kwargs)
                
III. Decorate the class and pass the name of the method to be decorated e.g

		@method_decorator(login_required, name='dispatch')
		class ProtectedView(TemplateView):
    	                template_name = 'secret.html'
    
For grouped common common decorators

	decorators = ['permission 1','permission2'...]
	@method_decorator(decorators, name='method_name')
	def dispatch(self, *args, **kwargs):
		return super(ProtectedView, self).dispatch(*args, **kwargs)
                
Multiple decorators can also be passed to a CBV as below

	@method_decorator(never_cache, name='dispatch')
	@method_decorator(login_required, name='dispatch')
	class ProtectedView(TemplateView):
		template_name = 'secret.html'

So, In your next Django project try to explore the goodness of CBVs. You'll be glad you did.

**Happy Coding**!
