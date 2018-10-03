##欢迎

使用pandas心得

'''一、最简单的形式，说明原理'''
    df=DataFrame([4])
    df=DataFrame([4],index=['A'],columns=['b'])
    df=DataFrame({'b':4},index=['A'])
    '''特例：纵向重复'''
    df=DataFrame({'b':4},index=['A','B'])
  
    '''二、1、两个数据：说明方向：axis=1'''
    df=DataFrame([4,2],index=['a','b'],columns=['A'])
    df=DataFrame({'A':[4,2]},index=['a','b'])
    '''二、2、两个数据：axis=0'''
    df=DataFrame([[4,2]],index=['a'],columns=['A','B'])
    df=DataFrame([{'A':4,'B':2}],index=['a'])
    '''特例：纵向重复'''
    df=DataFrame([{'A':4,'B':2}],index=['a','b'])
  
    '''三、默认形式'''
    df=DataFrame([[14,2],[3,5]],index=['a','b'],columns=['A','B'])
    df=DataFrame({'A':[14,2],'B':[3,5]},index=['a','b'])
    
    df = pd.DataFrame({'A': {0: 'a', 1: 'b', 2: 'c'},
                       'B': {0: 1, 1: 3, 2: 5},
                       'C': {0: 2, 1: 4, 2: 6}})
    
    '''没有特例'''
    df=DataFrame([{'A':[4,2],'B':[3,5]}],index=['a','b'])

    '''四、MultiIndex:'''
    '''    1、多级index'''
    '''最简单多级index:展开形式，生成方式_1'''
    arrays = [['bar', 'bar', 'baz', 'baz', 'foo', 'foo', 'qux', 'qux'],
              ['one', 'two', 'one', 'two', 'one', 'two', 'one', 'two']]
    '''增加name'''
    tuples = list(zip(*arrays))
    index = pd.MultiIndex.from_tuples(tuples, names=['first', 'second'])
   
    '''最简单多级index:生成方式  2'''
    iterables = [['bar', 'baz', 'foo', 'qux'], ['one', 'two']]
    index=pd.MultiIndex.from_product(iterables, names=['first', 'second'])
 
    '''两种区别：index有无name'''
#    index=pd.MultiIndex.from_product(iterables)
    
    df = pd.DataFrame(np.random.randn(8, 4), index=index)
#    
    '''    2、多级columns'''
    df = pd.DataFrame(np.random.randn(3, 8), index=['A', 'B', 'C'], columns=index)
    
    '''    3、多级index+多级columns'''
    df=pd.DataFrame(np.random.randn(6, 6), index=index[:6], columns=index[:6])
#    
    '''显示方式设置'''
    pd.set_option('display.multi_sparse', False)
    pd.set_option('display.multi_sparse', True)
#    print(df)
    print()
    '''返回数值'''
    '''index'''
#    print(df.index.get_level_values(0))
#    print(df.index.get_level_values('second'))
    '''columns'''
#    print(df['bar'])
#    
    '''返回name有区别'''
    '''columns'''
    print(df['bar', 'one'])
    print(df['bar']['one'])
#    
    '''由报表产生步骤：      A  B  C  D
    first second third            
    bar   one    1      4  1  8  9
          two    1      7  5  7  0
    baz   one    1      6  6  8  0
          three  2      5  3  5  3'''
    '''第二步、1、做出index'''
    arrays = [['bar', 'bar', 'baz', 'baz',],['one', 'two', 'one', 'three'],[1,1,1,2]]      
    tuples = list(zip(*arrays))
    #print(tuples)
    '''第二步、2、做出MultiIndex、2'''
    index = pd.MultiIndex.from_tuples(tuples,)
    index = pd.MultiIndex.from_tuples(tuples, names=['first', 'second','third'])
    '''第一步，做出DataFrame'''
    df=DataFrame({'A':[4,7,6,5],'B':[1,5,6,3],'C':[8,5,8,5],'D':[9,0,0,3]})
    '''第三步，做出DataFrame'''
    df=DataFrame([[4,1,8,9],[7,5,7,0],[6,6,8,0],[5,3,5,3]],
                 columns=['A','B','C','D'],index=index)
    print(df)
    
    '''位置操作'''
    df = pd.DataFrame({'a': np.random.randn(6).astype('f4'),
           'b': [True, False] * 3,
           'c': [1.0, 2.0] * 3,
           'd': ['a', 5] * 3})
    print(df.dtypes)
    print(df.select_dtypes(include=['bool']))
    print(df.select_dtypes(include=['object']))
    print(df.select_dtypes(include=['float64']))
    print(df.select_dtypes(exclude=['floating']))
    
    print(df.values)
    print(df)
    print(df.axes)
    '''[RangeIndex(start=0, stop=6, step=1), Index(['a', 'b', 'c'], dtype='object')]'''
    
    print(df.ndim)
    #2
    print(df.size)
    #18
    print(df.shape)
    (6, 3)
    
    print(df.memory_usage(index=True, deep=False))
    
    '''df.insert(loc, column, value, allow_duplicates=False)'''
    print(df.insert(1,'a1', 2, allow_duplicates=False))
    print(df.insert(1,'a1', 2, allow_duplicates=True))
#    df.to_csv(r'H:\test\DataFrame2.csv')
    print()
    
    s = Series(pd.date_range('2012-1-1', periods=3, freq='D'))
    #print(s)
    td = Series([ pd.Timedelta(days=i) for i in range(3) ])
    dicte=dict(A = s, B = td)
    #print(dicte)
    
    df = DataFrame(dict(A = s, B = td))
    
#    df=df.set_index('A')
    #df.index=pd.to_datetime()
    #df.index=pd.to_datetime()
    print(df)
    print(df.index)
    '''间隔性'''
    index=pd.IntervalIndex.from_arrays([0, 1, 2], [1, 2, 3])
    
    '''from_items is deprecated'''
#    pd.DataFrame.from_items([('A', [1, 2, 3]), ('B', [4, 5, 6])],
#                             orient='index', columns=['one', 'two', 'three'])
    """
    pandas.crosstab(index, columns, 
                    values=None, 
                    rownames=None, colnames=None, 
                    aggfunc=None, margins=False, dropna=True, normalize=False)
    """
    a=np.array(['foo', 'foo', 'foo', 'foo', 'bar', 'bar',
       'bar', 'bar', 'foo', 'foo', 'foo'], dtype=object)
    b=np.array(['one', 'one', 'one', 'two', 'one', 'one',
       'one', 'two', 'two', 'two', 'one'], dtype=object)
    c=np.array(['dull', 'dull', 'shiny', 'dull', 'dull', 'shiny',
       'shiny', 'dull', 'shiny', 'shiny', 'shiny'], dtype=object)
    d=pd.crosstab(a, [b, c], rownames=['a'], colnames=['b', 'c'])
    print(a)
    print(d)
    print()
    foo = pd.Categorical(['a', 'b'], categories=['a', 'b', 'c'])
    bar = pd.Categorical(['d', 'e'], categories=['d', 'e', 'f'])
    print(foo)
    print(bar)
    fb=pd.crosstab(foo, bar)
    print(fb)

    print(df)
    for i, row in df.iterrows(): 
        print(row)
```


