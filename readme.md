A [bootstrapper](https://github.com/NancyFx/Nancy/wiki/Bootstrapper) implementation, for the [Nancy](http://nancyfx.org) framework, based on the Ninject inversion of control container.

## Usage

When Nancy detects that the `NinjectNancyBootstrapper` type is available in the AppDomain of your application, it will assume you want to use it, rather than the default one.

The easiest way to get the latest version of `NinjectNancyBootstrapper` in your application is to install the [`Nancy.Bootstrappers.Ninject` nuget package](http://www.nuget.org/packages/Nancy.Bootstrappers.Ninject/).

### Customizing

By inheriting from `NinjectNancyBootstrapper` you will gain access to the `IKernel` of the application and request containers and can perform what ever registrations that your application requires.

```c#
public class Bootstrapper : NinjectNancyBootstrapper
{
    protected override void ApplicationStartup(IKernel container, IPipelines pipelines)
    {
        // No registrations should be performed in here, however you may
        // resolve things that are needed during application startup.
    }

    protected override void ConfigureApplicationContainer(IKernel existingContainer)
    {
        // Perform registation that should have an application lifetime
    }

    protected override void ConfigureRequestContainer(IKernel container, NancyContext context)
    {
        // Perform registrations that should have a request lifetime
    }

    protected override void RequestStartup(IKernel container, IPipelines pipelines, NancyContext context)
    {
        // No registrations should be performed in here, however you may
        // resolve things that are needed during request startup.
    }
}
```

You can also override the `GetApplicationContainer` method and return a pre-existing container instance, instead of having Nancy create one for you. This is useful if Nancy is co-existing with another application and you want them to share a single container.

```c#
protected override IKernel GetApplicationContainer()
{
    // Return application container instance
}
```

You can also override the `CreateRequestContainer` method so that you could customize your child container. By default, the request container's binding scope is singleton as this would be the most common case.

```c#
protected override IKernel CreateRequestContainer()
{
    // Return request contaner instance
}
```

## Contributors

* [Andreas Håkansson](http://github.com/thecodejunkie)
* [Andy Pike](http://github.com/andypike)
* [Guido Tapia](http://github.com/gatapia)
* [Steven Robbins](http://github.com/grumpydev)

## Copyright

Copyright © 2010 Andreas Håkansson, Steven Robbins and contributors

## License

Nancy.Bootstrappers.Ninject is licensed under [MIT](http://www.opensource.org/licenses/mit-license.php "Read more about the MIT license form"). Refer to license.txt for more information.
