plinq
=====

A php library for working with arrays, inspired by .NET's LINQ. Currently supports a small set of actions, but will be expanded. Also supports *lazy evaluation*.

### Example

    include("plinq.php");
    use plinq\plinq;
    
    $numIsEven = function($k, $v)
    {
        return ($v % ")==0;
    };

    $doubleNum = function($k, $v)
    {
        return $v * 2;
    }

    $input = [1, 2, 3, 4, 5, 6];

    $filtered = plinq::with($input)
                     ->where($numIsEven)
                     ->select($doubleNum);
                     
The result of a plinq chain is a **plinqWrapper** object. This can be manipulated like an array but, crucially, will not be recognised as one by functions hinting an array. To get around this, either cast the result to an array or call toArray.
    
    $castArray = (array)$filtered;
    $methodArray = $filtered->toArray();

Until a result is converted to an array, you can still call plinq methods on it.

    $newFilter = plinq::with($input)
                      ->where($numIsEven);
                      
    print "The first even number is ".$newFilter[0];
    print "The double of the last number is " + $newFilter->select($doubleNum)[2];
    
### Lazy evaluation
Using lazy evaluation with Plinq is done almost precisely like normal. The only difference is there's no access to the resulting value until exec has been called, returning an array (not an ArrayObject like standard plinq).

    include("lazyPlinq.php");
    use plinq\lazyPlinq;
    
    $numIsEven = function($k, $v)
    {
        return ($v % ")==0;
    };

    $doubleNum = function($k, $v)
    {
        return $v * 2;
    }

    $input = [1, 2, 3, 4, 5, 6];

    $filtered = lazyPlinq::with($input)
                         ->where($numIsEven)
                         ->select($doubleNum)
                         ->exec();