
# TVRecyclerView
一个TV端的RecyclerView，采用GridLayoutManager布局。
1.自定义Item的根布局（InnerFocusLayout）
     是其item内部的按钮可以获取焦点，而item自身不可获取焦点。实现item内部view获取焦点时，设置item的选中。添加返回上层的选中项。
2.重写RecyclerView（InnerRecycelrView）：
 1.解决原生RecyclerView自动寻找焦点时的bug：
           在onFocusSearchFailed时，根据重写的规则，寻找下一个正确的焦点。
 2.解决由于focus的view为item的内部项，导致上下滑动时，item滑动不完整，只显示一部分的bug。
           requestChildFocus负责计算需要滑动的距离，调用smoothScrollBy实现滑动，以及设置view获取焦点。
            重写requestChildFocus（）中，测量item在RecyclerView的坐标，计算需要滑动的距离。
            需要注意，焦点移动分为，RecyclerView不滑动，和滑动。
            每次当焦点变化时，requestChildFocus（）都会调用，但不一定会调用smoothScrollBy（），
            如果仅仅在requestChildFocus，手动调用smoothScrollBy（）可以解决，RecylcerView没有整体滑动，但item不完整的情况。
            如RecyclerView整体需要滑动，才能展示下一个项（此时仍然不完整展示），可以在super.requestChildFocus()后，
            再次通过offsetDescendantRectToMyCoords获取item的坐标，然后判断是否需要继续滑动。
            =======  此处还有其他方案。比如不再super,直接重写其父类的所有方法，但需要考虑到的变量和私有方法拆出来的问题，
                 也可以予以尝试========
