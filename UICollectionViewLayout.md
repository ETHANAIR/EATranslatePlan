# 你真的会使用UICollectionViewLayout吗？


> 这阵子接到一个需求，让我对`UICollectionView`有了新的认识，促使我对它进行一遍系统的理解。
> 
> `UICollectionView`大家都已经很熟悉了，这次说一下`UICollectionViewLayout`


不知道你有没有认真的阅读[UICollectionViewLayout](https://developer.apple.com/documentation/uikit/uicollectionviewlayout)这个类的官方文档。反正我之前是没有很好的阅读并理解，只是单纯的会使用而已。

如果你英文不太好没关系，接下来我给你用正宗的普通话嗦一遍。

## UICollectionViewLayout

> An abstract base class for generating layout information for a collection view.

> 用于为集合视图生成布局信息的抽象基类

就是你要是想用`UICollectionView`，就必须得用它，知道不，不然你拿啥给`UICollectionViewCell`布局？什么？你说你要用`UICollectionViewFlowLayout`? 这玩意是`UICollectionViewLayout`的子类。所以把它爹搞定了之后你就可以随意玩了。

## Overview - 概览
> The job of a layout object is to determine the placement of cells, supplementary views, and decoration views inside the collection view’s bounds and to report that information to the collection view when asked. The collection view then applies the provided layout information to the corresponding views so that they can be presented onscreen.

> 布局对象的工作是确定集合视图边界内的单元格，补充视图和装饰视图的位置，并在被询问时将该信息报告给集合视图。集合视图然后将提供的布局信息应用于相应的视图，以便它们可以在屏幕上呈现。

> You must subclass UICollectionViewLayout in order to use it. Before you consider subclassing, though, you should look at the UICollectionViewFlowLayout class to see if it can be adapted to your layout needs.

> 您必须继承UICollectionViewLayout才能使用它。不过，在考虑子类化之前，您应该查看UICollectionViewFlowLayout类以查看它是否可以适应您的布局需求。

## Subclassing Notes - 子类化注意事项
> The main job of a layout object is to provide information about the position and visual state of items in the collection view. The layout object does not create the views for which it provides the layout. Those views are created by the collection view’s data source. Instead, the layout object defines the position and size of visual elements based on the design of the layout.

> 布局对象的主要工作是提供有关集合视图中项目的位置和视觉状态的信息。布局对象不会为其提供布局创建视图。这些视图是由集合视图的数据源创建的。相反，布局对象基于布局的设计来定义视觉元素的位置和大小。

> Collection views have three types of visual elements that need to be laid out:

> 集合视图有三种需要布置的视觉元素：

> * Cells are the main elements positioned by the layout. Each cell represents a single data item in the collection. A collection view can have a single group of cells or it can divide those cells into multiple sections. The layout object’s main job is to arrange the cells in the collection view’s content area.
> > 单元格是布局定位的主要元素。每个单元格表示集合中的单个数据项。集合视图可以包含一组单元，也可以将这些单元划分为多个部分。布局对象的主要工作是将单元格排列在集合视图的内容区域中。

> * Supplementary views present data but are different than cells. Unlike cells, supplementary views cannot be selected by the user. Instead, you use supplementary views to implement things like header and footer views for a given section or for the entire collection view. Supplementary views are optional and their use and placement is defined by the layout object.
> > 辅助视图显示数据，但不同于单元格。与单元格不同，辅助视图不能由用户选择。相反，您可以使用补充视图来为给定部分或整个集合视图实现诸如页眉和页脚视图之类的内容。辅助视图是可选的，它们的使用和布局由布局对象定义。

> * Decoration views are visual adornments that cannot be selected and are not inherently tied to the data of the collection view. Decoration views are another type of supplementary view. Like supplementary views, they are optional and their use and placement is defined by the layout object.
> > 装饰视图是不可选择的视觉装饰物，并且其本身并不依赖于集合视图的数据。装饰视图是另一种辅助视图。像辅助视图一样，它们是可选的，它们的使用和布局由布局对象定义。

> The collection view asks its layout object to provide layout information for these elements at many different times. Every cell and view that appears on screen is positioned using information from the layout object. Similarly, every time items are inserted into or deleted from the collection view, additional layout occurs for the items being added or removed. However, the collection view always limits layout to the objects that are visible onscreen.

> 集合视图会要求其布局对象在很多不同的时间为这些元素提供布局信息。显示在屏幕上的每个单元格和视图都使用布局对象中的信息进行定位。同样，每次项目被插入到集合视图或从集合视图中删除时，都会为添加或删除的项目进行额外的布局。但是，集合视图始终将布局限制为屏幕上可见的对象。


## Methods to Override
> Every layout object should implement the following methods:

> 每个布局对象都要实现下面的这几个方法

> * [collectionViewContentSize](https://developer.apple.com/documentation/uikit/uicollectionviewlayout/1617796-collectionviewcontentsize)

> * [layoutAttributesForElements(in:)](https://developer.apple.com/documentation/uikit/uicollectionviewlayout/1617769-layoutattributesforelements)

> * [layoutAttributesForItem(at:)](https://developer.apple.com/documentation/uikit/uicollectionviewlayout/1617797-layoutattributesforitem)

> * [layoutAttributesForSupplementaryView(ofKind:at:) (if your layout supports supplementary views)](https://developer.apple.com/documentation/uikit/uicollectionviewlayout/1617792-layoutattributesforsupplementary)

> * [layoutAttributesForDecorationView(ofKind:at:) (if your layout supports decoration views)](https://developer.apple.com/documentation/uikit/uicollectionviewlayout/1617809-layoutattributesfordecorationvie)

> * [shouldInvalidateLayout(forBoundsChange:)](https://developer.apple.com/documentation/uikit/uicollectionviewlayout/1617781-shouldinvalidatelayout)

> When the data in the collection view changes and items are to be inserted or deleted, the collection view asks its layout object to update the layout information. Specifically, any item that is moved, added, or deleted must have its layout information updated to reflect its new location. For moved items, the collection view uses the standard methods to retrieve the item’s updated layout attributes. For items being inserted or deleted, the collection view calls some different methods, which you should override to provide the appropriate layout information:
> 
> 当集合视图中的数据发生更改并且要插入或删除项目时，集合视图会要求其布局对象更新布局信息。具体而言，任何移动，添加或删除的项目都必须更新其布局信息以反映其新位置。对于移动的项目，集合视图使用标准方法来检索项目的更新布局属性。对于正在插入或删除的项目，集合视图会调用一些不同的方法，您应该重写以提供相应的布局信息：

> * initialLayoutAttributesForAppearingItem(at:)

> * initialLayoutAttributesForAppearingSupplementaryElement(ofKind:at:)

> * initialLayoutAttributesForAppearingDecorationElement(ofKind:at:)

> * finalLayoutAttributesForDisappearingItem(at:)

> * finalLayoutAttributesForDisappearingSupplementaryElement(ofKind:at:)

> * finalLayoutAttributesForDisappearingDecorationElement(ofKind:at:)

> In addition to these methods, you can also override the [prepare(forCollectionViewUpdates:)](https://developer.apple.com/documentation/uikit/uicollectionviewlayout/1617784-prepare) to handle any layout-related preparation. You can also override the [finalizeCollectionViewUpdates()](https://developer.apple.com/documentation/uikit/uicollectionviewlayout/1617787-finalizecollectionviewupdates) method and use it to add animations to the overall animation block or to implement any final layout-related tasks.

## Optimizing Layout Performance Using Invalidation Contexts - 使用'无效上下文'优化布局性能
> When designing your custom layouts, you can improve performance by invalidating only those parts of your layout that actually changed. When you change items, calling the invalidateLayout() method forces the collection view to recompute all of its layout information and reapply it. A better solution is to recompute only the layout information that changed, which is exactly what invalidation contexts allow you to do. An invalidation context lets you specify which parts of the layout changed. The layout object can then use that information to minimize the amount of data it recomputes.
> 
> 在设计自定义布局时，可以通过仅使布局中实际更改的那些部分无效来提高性能。当您更改项目时，调用invalidateLayout（）方法会强制集合视图重新计算其所有布局信息并重新应用它。更好的解决方案是只重新计算已更改的布局信息，这正是无效环境允许您执行的操作。无效上下文让您指定布局的哪些部分发生了变化。布局对象然后可以使用该信息来最小化其重新计算的数据量。

> To define a custom invalidation context for your layout, subclass the [UICollectionViewLayoutInvalidationContext](https://developer.apple.com/documentation/uikit/uicollectionviewlayoutinvalidationcontext) class. In your subclass, define custom properties that represent the parts of your layout data that can be recomputed independently. When you need to invalidate your layout at runtime, create an instance of your invalidation context subclass, configure the custom properties based on what layout information changed, and pass that object to your layout’s [invalidateLayout（with :)](https://developer.apple.com/documentation/uikit/uicollectionviewlayout/1617771-invalidatelayout) method. Your custom implementation of that method can use the information in the invalidation context to recompute only the portions of your layout that changed.
> 
> 要为布局定义自定义无效上下文，请为[UICollectionViewLayoutInvalidationContext](https://developer.apple.com/documentation/uikit/uicollectionviewlayoutinvalidationcontext)类继承子类。在你的子类中，定义代表可以独立重新计算的布局数据部分的自定义属性。当您需要在运行时使布局失效时，创建失效上下文子类的实例，根据更改的布局信息配置自定义属性，并将该对象传递给布局的[invalidateLayout（with :)](https://developer.apple.com/documentation/uikit/uicollectionviewlayout/1617771-invalidatelayout)方法。您对该方法的自定义实现可以使用失效上下文中的信息来仅重新计算已更改的布局部分。

> If you define a custom invalidation context class for your layout object, you should also override the invalidationContextClass method and return your custom class. The collection view always creates an instance of the class you specify when it needs an invalidation context. Returning your custom subclass from this method ensures that your layout object always has the invalidation context it expects.
> 
> 如果为布局对象定义了自定义无效上下文类，则还应该覆盖[invalidationContextClass](https://developer.apple.com/documentation/uikit/uicollectionviewlayout/1617790-invalidationcontextclass)方法并返回您的自定义类。集合视图总是创建您需要无效上下文时指定的类的实例。从此方法返回自定义子类可确保您的布局对象始终具有预期的无效上下文。

###  Initializing the Collection View - 初始化集合视图布局对象
```
init()
```
> Initializes the collection view layout object.

> 初始化集合视图布局对象

```
init?(coder: NSCoder)
```

###  Getting the Collection View Information - 获取集合视图的信息
```
var collectionView: UICollectionView?
```
> The collection view object currently using this layout object.

> 持有当前布局对象的集合视图对象

```
var collectionViewContentSize: CGSize
```
> Returns the width and height of the collection view’s contents. 
> 
> Subclasses must override this method and use it to return the width and height of the collection view’s content. These values represent the width and height of all the content, not just the content that is currently visible. The collection view uses this information to configure its own content size for scrolling purposes.

> 返回集合视图内容的宽高。
> 
> 子类必须重写此方法并使用它返回集合视图内容的宽度和高度。这些值代表所有内容的宽度和高度，而不仅仅是当前可见的内容。集合视图使用此信息为滚动目的配置其自己的内容大小。

### Providing Layout Attributes - 提供布局属性

```
class var layoutAttributesClass: AnyClass
```
> Returns the class to use when creating layout attributes objects.
> 
> 返回创建布局属性对象时要使用的类.

> If you subclass UICollectionViewLayoutAttributes in order to manage additional layout attributes, you should override this method and return your custom subclass. The methods for creating layout attributes use this class when creating new layout attributes objects.
> 
> 如果您继承UICollectionViewLayoutAttributes以管理其他布局属性，则应该重写此方法并返回您的自定义子类。创建布局属性对象时，创建布局属性的方法使用此类。

> This method is intended for subclassers only and does not need to be called by your code.
> 此方法仅适用于子类，不需要由您的代码调用。

```
func prepare()
```
> Tells the layout object to update the current layout.
> 
> 告诉布局对象更新当前布局。
> 
> Layout updates occur the first time the collection view presents its content and whenever the layout is invalidated explicitly or implicitly because of a change to the view. During each layout update, the collection view calls this method first to give your layout object a chance to prepare for the upcoming layout operation.
> 
> 布局更新发生在集合视图第一次显示其内容时以及布局由于视图更改而显式或隐式地失效时。在每次布局更新期间，集合视图首先调用此方法，以使您的布局对象有机会为即将到来的布局操作做好准备。

> The default implementation of this method does nothing. Subclasses can override it and use it to set up data structures or perform any initial computations needed to perform the layout later.
> 
> 此方法的默认实现不做任何事情。子类可以覆盖它并使用它来设置数据结构或执行以后执行布局所需的任何初始计算。

```
func layoutAttributesForElements(in: CGRect)
```
> Returns the layout attributes for all of the cells and views in the specified rectangle.
> 
> 返回指定矩形中所有单元格和视图的布局属性。
> 
> Subclasses must override this method and use it to return layout information for all items whose view intersects the specified rectangle. Your implementation should return attributes for all visual elements, including cells, supplementary views, and decoration views.
> 
> 子类必须重写此方法并使用它来返回其视图与指定矩形相交的所有项目的布局信息。你的实现应该返回所有视觉元素的属性，包括单元格，辅助视图和装饰视图。

> When creating the layout attributes, always create an attributes object that represents the correct element type (cell, supplementary, or decoration). The collection view differentiates between attributes for each type and uses that information to make decisions about which views to create and how to manage them.
> 
> 创建布局属性时，请始终创建一个代表正确元素类型（单元格，辅助或装饰）的属性对象。集合视图区分每种类型的属性，并使用该信息来决定要创建哪些视图以及如何管理它们。

```
func layoutAttributesForItem(at: IndexPath)
```

> Returns the layout attributes for the item at the specified index path.
> 
> 返回指定索引路径中项目的布局属性。
> 
> Subclasses must override this method and use it to return layout information for items in the collection view. You use this method to provide layout information only for items that have a corresponding cell. Do not use it for supplementary views or decoration views.
> 
> 子类必须重写此方法并使用它来返回集合视图中项目的布局信息。您可以使用此方法仅为具有相应单元格的项目提供布局信息。请勿将其用于补充视图或装饰视图。

```
func layoutAttributesForInteractivelyMovingItem(at: IndexPath, 
withTargetPosition: CGPoint)
```
Returns the layout attributes of an item when it is being moved interactively by the user.

```
func layoutAttributesForSupplementaryView(ofKind: String, at: IndexPath)
```

> Returns the layout attributes for the specified supplementary view.
> 
> 返回指定辅助视图的布局属性。
> 
> If your layout object defines any supplementary views, you must override this method and use it to return layout information for those views.
> 
> 如果您的布局对象定义了任何补充视图，则必须重写此方法并使用它返回这些视图的布局信息。

```
func layoutAttributesForDecorationView(ofKind: String, at: IndexPath)
```
> Returns the layout attributes for the specified decoration view.
> 
> 返回指定装饰视图的布局属性。
> 
> If your layout object defines any decoration views, you must override this method and use it to return layout information for those views.
> 
> 如果您的布局对象定义了任何装饰视图，则必须重写此方法并使用它来返回这些视图的布局信息。

```
func targetContentOffset(forProposedContentOffset: CGPoint)
```
> Returns the content offset to use after an animated layout update or change.
> 
> 返回动画布局更新或更改后使用的内容偏移量。

> During layout updates, or when transitioning between layouts, the collection view calls this method to give you the opportunity to change the proposed content offset to use at the end of the animation. You might override this method if the animations or transition might cause items to be positioned in a way that is not optimal for your design.
> 
> 在布局更新期间，或在布局之间转换时，集合视图会调用此方法，以便您有机会更改要在动画结尾处使用的建议内容偏移量。如果动画或转换可能导致项目的定位方式不适合您的设计，那么您可以重写此方法。

> The collection view calls this method after calling the prepare() and collectionViewContentSize methods.
> 
> 调用prepare()和collectionViewContentSize方法后，集合视图调用此方法。

```
func targetContentOffset(forProposedContentOffset: CGPoint, withScrollingVelocity: CGPoint)
```
> Returns the point at which to stop scrolling.
> 
> 返回停止滚动的点。

> If you want the scrolling behavior to snap to specific boundaries, you can override this method and use it to change the point at which to stop. For example, you might use this method to always stop scrolling on a boundary between items, as opposed to stopping in the middle of an item.
> 
> 如果您希望滚动行为捕捉到特定边界，则可以覆盖此方法并使用它来更改停止点。例如，您可以使用此方法始终停止在项目之间的边界上滚动，而不是停止在项目的中间。
> 
> When items are inserted or deleted, the collection view notifies its layout object so that it can adjust the layout as needed. The first step in that process is to call this method to let the layout object know what changes to expect. After that, additional calls are made to gather layout information for inserted, deleted, and moved items that are going to be animated around the collection view.
> 
> 当插入或删除项目时，集合视图会通知其布局对象，以便它可以根据需要调整布局。该过程的第一步是调用此方法让布局对象知道需要进行哪些更改。之后，还会调用额外的电话来收集插入，删除和移动项目的布局信息，这些项目将围绕集合视图进行动画处理。


### Responding to Collection View Updates
```
func prepare(forCollectionViewUpdates: [UICollectionViewUpdateItem])
```
> Notifies the layout object that the contents of the collection view are about to change.
> 
> 通知布局对象集合视图的内容即将更改。

```
func finalizeCollectionViewUpdates()
```
> Performs any additional animations or clean up needed during a collection view update.
> 
> 在集合视图更新期间执行所需的其他动画或清理。
> 
> The collection view calls this method as the last step before proceeding to animate any changes into place. This method is called within the animation block used to perform all of the insertion, deletion, and move animations so you can create additional animations using this method as needed. Otherwise, you can use it to perform any last minute tasks associated with managing your layout object’s state information.
> 
> 集合视图在将任何更改动画到位之前将其作为最后一步调用。在用于执行所有插入，删除和移动动画的动画块中调用此方法，以便您可以根据需要使用此方法创建其他动画。否则，您可以使用它来执行与管理布局对象的状态信息相关的任何最后时刻的任务。

```
func indexPathsToInsertForSupplementaryView(ofKind: String)
```
> Returns an array of index paths for the supplementary views you want to add to the layout.
> 
> 返回要添加到布局的补充视图的索引路径数组。
> 
> The collection view calls this method whenever you add cells or sections to the collection view. Implementing this method gives your layout object an opportunity to add new supplementary views to complement the additions.
> 
> 每当向集合视图添加单元格或节时，集合视图都会调用此方法。实现这个方法可以让你的布局对象有机会添加新的补充视图来补充附加内容。

> The collection view calls this method between its calls to prepare(forCollectionViewUpdates:) and finalizeCollectionViewUpdates().
> 
> 集合视图在调用prepare（forCollectionViewUpdates :)和finalizeCollectionViewUpdates（）之间调用此方法。

```
func indexPathsToInsertForDecorationView(ofKind: String)
```
> Returns an array of index paths representing the decoration views to add.
> 
> 返回表示要添加的装饰视图的索引路径数组。
> 
> The collection view calls this method whenever you add cells or sections to the collection view. Implementing this method gives your layout object an opportunity to add new decoration views to complement the additions.
> 
> 每当向集合视图添加单元格或节时，集合视图都会调用此方法。实现这种方法可以使布局对象有机会添加新的装饰视图以补充附加内容。

> The collection view calls this method between its calls to prepare(forCollectionViewUpdates:) and finalizeCollectionViewUpdates().
> 
> 集合视图在调用prepare（forCollectionViewUpdates :)和finalizeCollectionViewUpdates（）之间调用此方法。

```
func initialLayoutAttributesForAppearingItem(at: IndexPath)
```
> Returns the starting layout information for an item being inserted into the collection view.
> 
> 返回插入到集合视图中的项目的开始布局信息。
> 
> This method is called after the prepare(forCollectionViewUpdates:) method and before the finalizeCollectionViewUpdates() method for any items that are about to be inserted. Your implementation should return the layout information that describes the initial position and state of the item. The collection view uses this information as the starting point for any animations. (The end point of the animation is the item’s new location in the collection view.) If you return nil, the layout object uses the item’s final attributes for both the start and end points of the animation.
> 
> 此方法在prepare（forCollectionViewUpdates :)方法之后和即将插入任何项目的finalizeCollectionViewUpdates（）方法之前调用。您的实现应返回描述项目初始位置和状态的布局信息。收藏视图使用此信息作为任何动画的起点。 （动画的结束点是项目在集合视图中的新位置。）如果返回nil，则布局对象将该项目的最终属性用于动画的开始点和结束点。

> The default implementation of this method returns nil.
> 
> 此方法的默认实现返回nil。

```
func initialLayoutAttributesForAppearingSupplementaryElement(ofKind: String, at: IndexPath)
```
> Returns the starting layout information for a supplementary view being inserted into the collection view.
> 
> 返回插入到集合视图中的补充视图的起始布局信息。
> 
> This method is called after the prepare(forCollectionViewUpdates:) method and before the finalizeCollectionViewUpdates() method for any supplementary views that are about to be inserted. Your implementation should return the layout information that describes the initial position and state of the view. The collection view uses this information as the starting point for any animations. (The end point of the animation is the view’s new location in the collection view.) If you return nil, the layout object uses the item’s final attributes for both the start and end points of the animation.
> 
> 此方法在prepare（forCollectionViewUpdates :)方法之后以及在即将插入的任何补充视图的finalizeCollectionViewUpdates（）方法之前调用。你的实现应该返回描述视图初始位置和状态的布局信息。收藏视图使用此信息作为任何动画的起点。 （动画的结束点是视图在集合视图中的新位置。）如果返回nil，则布局对象将为该动画的开始点和结束点使用项目的最终属性。

> The default implementation of this method returns nil.
> 
> 此方法的默认实现返回nil。

```
func initialLayoutAttributesForAppearingDecorationElement(ofKind: String, at: IndexPath)
```
> Returns the starting layout information for a decoration view being inserted into the collection view.
> 
> 返回插入到集合视图中的修饰视图的开始布局信息。
> 
> This method is called after the prepare(forCollectionViewUpdates:) method and before the finalizeCollectionViewUpdates() method for any decoration views that are about to be inserted. Your implementation should return the layout information that describes the initial position and state of the view. The collection view uses this information as the starting point for any animations. (The end point of the animation is the view’s new location in the collection view.) If you return nil, the layout object uses the item’s final attributes for both the start and end points of the animation.
> 
> 此方法在prepare（forCollectionViewUpdates :)方法之后和即将插入的装饰视图的finalizeCollectionViewUpdates（）方法之前调用。你的实现应该返回描述视图初始位置和状态的布局信息。收藏视图使用此信息作为任何动画的起点。 （动画的结束点是视图在集合视图中的新位置。）如果返回nil，则布局对象将为该动画的开始点和结束点使用项目的最终属性。
> 
> The default implementation of this method returns nil.
> 
> 此方法的默认实现返回nil。

```
func indexPathsToDeleteForSupplementaryView(ofKind: String)
```
> Returns an array of index paths representing the supplementary views to remove.
> 
> 返回表示要删除的补充视图的索引路径数组。
> 
> The collection view calls this method whenever you delete cells or sections to the collection view. Implementing this method gives your layout object an opportunity to remove any supplementary views that are no longer needed.
> 
> 无论何时将集合视图的单元格或部分删除，集合视图都会调用此方法。实现这种方法可以让布局对象有机会去除不再需要的任何补充视图。
> 
> The collection view calls this method between its calls to prepare(forCollectionViewUpdates:) and finalizeCollectionViewUpdates().
> 
> 集合视图在调用prepare（forCollectionViewUpdates :)和finalizeCollectionViewUpdates（）之间调用此方法。

```
func indexPathsToDeleteForDecorationView(ofKind: String)
```
> Returns an array of index paths representing the decoration views to remove.
> 
> 返回表示要删除的装饰视图的索引路径数组。
> 
> The collection view calls this method whenever you delete cells or sections to the collection view. Implementing this method gives your layout object an opportunity to remove any decoration views that are no longer needed.
> 
> 无论何时将集合视图的单元格或部分删除，集合视图都会调用此方法。实现这种方法可以让布局对象有机会去除不再需要的装饰视图。
> 
> The collection view calls this method between its calls to prepare(forCollectionViewUpdates:) and finalizeCollectionViewUpdates().
> 
> 集合视图在调用prepare（forCollectionViewUpdates :)和finalizeCollectionViewUpdates（）之间调用此方法。

```
func finalLayoutAttributesForDisappearingItem(at: IndexPath)
```
> Returns the final layout information for an item that is about to be removed from the collection view.
> 
> 返回即将从集合视图中移除的项目的最终布局信息。
> 
> This method is called after the prepare(forCollectionViewUpdates:) method and before the finalizeCollectionViewUpdates() method for any items that are about to be deleted. Your implementation should return the layout information that describes the final position and state of the item. The collection view uses this information as the end point for any animations. (The starting point of the animation is the item’s current location.) If you return nil, the layout object uses the same attributes for both the start and end points of the animation.
> 
> 在准备（forCollectionViewUpdates :)方法之后以及在即将删除的项目的finalizeCollectionViewUpdates（）方法之前调用此方法。你的实现应该返回描述项目最终位置和状态的布局信息。收集视图使用此信息作为任何动画的终点。（动画的起点是项目的当前位置。）如果返回nil，则布局对象对动画的开始点和结束点使用相同的属性。
> 
> The default implementation of this method returns nil.
> 
> 该方法默认实现返回nil

```
func finalLayoutAttributesForDisappearingSupplementaryElement(ofKind: String, at: IndexPath)
```
> Returns the final layout information for a supplementary view that is about to be removed from the collection view.
> 
> 返回即将从集合视图中移除的补充视图的最终布局信息。
> 
> This method is called after the prepare(forCollectionViewUpdates:) method and before the finalizeCollectionViewUpdates() method for any supplementary views that are about to be deleted. Your implementation should return the layout information that describes the final position and state of the view. The collection view uses this information as the end point for any animations. (The starting point of the animation is the view’s current location.) If you return nil, the layout object uses the same attributes for both the start and end points of the animation.
> 
> 此方法在prepare（forCollectionViewUpdates :)方法之后以及在将要删除的任何补充视图的finalizeCollectionViewUpdates（）方法之前调用。你的实现应该返回描述视图最终位置和状态的布局信息。收集视图使用此信息作为任何动画的终点。（动画的起点是视图的当前位置。）如果返回nil，则布局对象对动画的开始点和结束点使用相同的属性。
> 
> The default implementation of this method returns nil.
> 
> 该方法默认实现返回nil

```
func finalLayoutAttributesForDisappearingDecorationElement(ofKind: String, at: IndexPath)
```
> Returns the final layout information for a decoration view that is about to be removed from the collection view.
> 
> 返回即将从集合视图中删除的修饰视图的最终布局信息。
> 
> This method is called after the prepare(forCollectionViewUpdates:) method and before the finalizeCollectionViewUpdates() method for any decoration views that are about to be deleted. Your implementation should return the layout information that describes the final position and state of the view. The collection view uses this information as the end point for any animations. (The starting point of the animation is the view’s current location.) If you return nil, the layout object uses the same attributes for both the start and end points of the animation.
> 
> 此方法在prepare（forCollectionViewUpdates :)方法之后以及在将要删除的装饰视图的finalizeCollectionViewUpdates（）方法之前调用。你的实现应该返回描述视图最终位置和状态的布局信息。收集视图使用此信息作为任何动画的终点。 （动画的起点是视图的当前位置。）如果返回nil，则布局对象对动画的开始点和结束点使用相同的属性。
> 
> The default implementation of this method returns nil.
> 
> 该方法默认实现返回nil

```
func targetIndexPath(forInteractivelyMovingItem: IndexPath, withPosition: CGPoint)
```
> Returns the index path to for an item when it is at the specified location in the collection view’s bounds.
> 
> 当项目位于集合视图边界中的指定位置时，返回项目的索引路径。
> 
> During interactive movement of an item, this method maps points in the collection view’s bounds rectangle to index paths that correspond to the locations of those points. The default implementation of this method searches for an existing cell at the specified location and returns the index path of that cell. If there are multiple cells at the same location, the method returns the topmost cell—that is, the cell whose zIndex layout attribute value is greatest.
> 
> 在项目的交互式移动过程中，此方法将集合视图的边界矩形中的点映射到与这些点的位置相对应的索引路径。此方法的默认实现在指定位置搜索现有单元并返回该单元的索引路径。如果在同一位置有多个单元格，则该方法返回最上面的单元格 - 即zIndex布局属性值最大的单元格。
> 
> You can override this method as needed to change how the index path is determined. For example, you might return the index path of the cell that has the lowest zIndex value instead of the highest. If you override this method, you do not need to call super.
> 
> 您可以根据需要重写此方法以更改索引路径的确定方式。例如，您可能会返回具有最低zIndex值而非最高值的单元格的索引路径。如果您重写此方法，则不需要调用super。

### Invalidating the Layout
```
func invalidateLayout()
```
> Invalidates the current layout and triggers a layout update.

```
func invalidateLayout(with: UICollectionViewLayoutInvalidationContext)
```
> Invalidates the current layout using the information in the provided context object.

```
class var invalidationContextClass: AnyClass
```
> Returns the class to use when creating an invalidation context for the layout.

```
func shouldInvalidateLayout(forBoundsChange: CGRect)
```
> Asks the layout object if the new bounds require a layout update.
> 
> 如果新边界需要布局更新，则询问布局对象。
> 
> The default implementation of this method returns false. Subclasses can override it and return an appropriate value based on whether changes in the bounds of the collection view require changes to the layout of cells and supplementary views.
> 
> 此方法的默认实现返回false。子类可以覆盖它并根据集合视图的边界中的更改是否需要更改单元格和补充视图的布局来返回适当的值。

> These methods provide the fundamental layout information that the collection view needs to place contents on the screen. Of course, if your layout does not support supplementary or decoration views, do not implement the corresponding methods.
> 
> 这些方法提供了收集视图在屏幕上放置内容所需的基本布局信息。当然，如果您的布局不支持辅助或装饰视图，请不要实施相应的方法。

```
func invalidationContext(forBoundsChange: CGRect)
```
> Returns a context object that defines the portions of the layout that should change when a bounds change occurs.

```
func shouldInvalidateLayout(forPreferredLayoutAttributes: UICollectionViewLayoutAttributes, withOriginalAttributes: UICollectionViewLayoutAttributes)
```
> Asks the layout object if changes to a self-sizing cell require a layout update.

```
func invalidationContext(forPreferredLayoutAttributes: UICollectionViewLayoutAttributes, withOriginalAttributes: UICollectionViewLayoutAttributes)
```
> Returns a context object that identifies the portions of the layout that should change in response to dynamic cell changes.

```
func invalidationContext(forInteractivelyMovingItems: [IndexPath], withTargetPosition: CGPoint, previousIndexPaths: [IndexPath], previousPosition: CGPoint)
```
> Returns a context object that identifies the items that are being interactively moved in the layout.

```
func invalidationContextForEndingInteractiveMovementOfItems(toFinalIndexPaths: [IndexPath], previousIndexPaths: [IndexPath], movementCancelled: Bool)
```
> Returns a context object that identifies the items that were moved

### Coordinating Animated Changes
```
func prepare(forAnimatedBoundsChange: CGRect)
```
> Prepares the layout object for animated changes to the view’s bounds or the insertion or deletion of items.

```
func finalizeAnimatedBoundsChange()
```
> Cleans up after any animated changes to the view’s bounds or after the insertion or deletion of items.

### Transitioning Between Layouts
```
func prepareForTransition(from: UICollectionViewLayout)
```
> Tells the layout object to prepare to be installed as the layout for the collection view.

```
func prepareForTransition(to: UICollectionViewLayout)
```
> Tells the layout object that it is about to be removed as the layout for the collection view.

```
func finalizeLayoutTransition()
```
> Tells the layout object to perform any final steps before the transition animations occur.

### Registering Decoration Views
```
func register(AnyClass?, forDecorationViewOfKind: String)
```
> Registers a class for use in creating decoration views for a collection view.

```
func register(UINib?, forDecorationViewOfKind: String)
```
> Registers a nib file for use in creating decoration views for a collection view.

### Supporting Right-To-Left Layouts-左右手使用习惯的布局
```
var developmentLayoutDirection: UIUserInterfaceLayoutDirection
```
> The direction of the language you used when designing your custom layout.
> 
> 您在设计自定义布局时使用的左右手操作习惯
> 
> The default value of this property is the layout direction used by the language associated with the main bundle's development region. Subclasses may override this property and return a different value.
> 
> 此属性的默认值是与主包的开发区域关联的左右手使用习惯的布局方向。子类可以覆盖此属性并返回不同的值。

```
var flipsHorizontallyInOppositeLayoutDirection: Bool
```
> A Boolean value indicating whether the horizontal coordinate system is automatically flipped at appropriate times.
> 
> 一个布尔值，指示水平坐标系是否在适当的时间自动翻转。
> 
> The language you use during development naturally affects the decisions you make when configuring your layout object. When you develop using a left-to-right language, your layout information automatically matches the collection view's natural coordinate system. However, when the user's language has a right-to-left orientation, the layout information you provide is still based on the collection view's natural coordinate system. This discrepancy can cause layout issues for languages using the opposite orientation. When this property is set to true, the collection view automatically flips the orientation of its horizontal coordinate system to match the leading edge of the current language. (The developmentLayoutDirection property specifies the layout direction used to design the layout.) Flipping the horizontal coordinate system effectively flips your existing layout information, which should result in a better looking layout.
> 
> 在开发过程中使用的语言自然会影响您在配置布局对象时所做的决定。当您使用从左到右的方式进行开发时，布局信息会自动匹配收集视图的自然坐标系。但是，当用户习惯是从右到左的方向时，您提供的布局信息仍然基于集合视图的自然坐标系。这种差异会导致使用相反方向的习惯的布局问题。当此属性设置为true时，集合视图会自动翻转其水平坐标系的方向以匹配当前习惯的前沿。 （developmentLayoutDirection属性指定用于设计布局的布局方向。）翻转水平坐标系可以有效地翻转现有的布局信息，这会使布局更加美观。
> 
> The default value of this property is false.
> 
> 这个属性的默认值是false







