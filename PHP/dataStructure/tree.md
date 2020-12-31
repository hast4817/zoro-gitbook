# php实现树



效果(二叉树):
```php
/* br的节点为[1,null,2,null,null,3] */
$br = new BinaryTree();
$br->add(1);
$br->add(null);
$br->add(2);
$br->add(3);
```

Node类(节点实现)
```php
/**
 * 节点实现
 */
class Node
{
	/**
	 * 节点值
	 * @var
	 */
	public $val;

	/**
	 * 左孩子
	 * @var
	 */
	public $left;

	/**
	 * 右孩子
	 * @var
	 */
	public $right;

	/**
	 * Node constructor.
	 * @param $item
	 */
	public function __construct($val = -1,$left = null,$rchild =  null)
	{
		$this->val = $val;
		$this->left = $left;
		$this->right = $rchild;
	}
}
```
BinaryTree类实现
```php
<?php

/**
 * 二叉树基本操作
 * 1.add(val)       添加节点
 * 2.breadth()       层次遍历
 * 3.preorder(node)  先序遍历
 * 4.inorder(node)   中序遍历
 * 5.postorder(node) 后序遍历
 */
class BinaryTree
{
	/**
	 * 根节点
	 * @var null
	 */
	public $root;

	/**
	 * Tree constructor.
	 * @param null $root
	 */
	public function __construct($root = null)
	{
		$this->root = $root;
	}

	/**
	 * 节点添加
	 * @param $elem
	 * @return void
	 */
	public function add($elem)
	{
		$node = new Node($elem);

		//若树是空的，则队根节点赋值
		if(is_null($this->root)){
			$this->root = $node;
			return;
		}

		$queue = [];
		array_push($queue,$this->root);

		//对已有节点进行层次遍历
		while($queue){
			//出队
			$cur = array_shift($queue);

			//左孩子处理
			if(is_null($cur->left)){
				$cur->left = $node;
				return;
			}else{
				!is_null($cur->left->val) && array_push($queue,$cur->left);
			}

			//右孩子处理
			if(is_null($cur->right)){
				$cur->right = $node;
				return;
			}else{
				!is_null($cur->right->val) && array_push($queue,$cur->right);
			}
		}

	}

	/**
	 * 层次遍历
	 * 从根开始，从上到下、从左到右遍历整个树的节点
	 */
	public function breadth()
	{
		if(is_null($this->root)){
			return;
		}
		$queue = [$this->root];
		while ($queue){
			$cur = array_shift($queue);
			echo $cur->val . '<br />';
			if(!is_null($cur->left)){
				array_push($queue,$cur->left);
			}
			if(!is_null($cur->right)){
				array_push($queue,$cur->right);
			}
		}
	}

	/**
	 * 先序遍历
	 * 根节点->左子树->右子树
	 * @param Node $node
	 */
	public function preorder($node)
	{
		if(is_null($node)){
			return;
		}
		echo $node->val . '<br />';
		$this->preorder($node->left);
		$this->preorder($node->right);
	}

	/**
	 * 中序遍历
	 * 左子树->根节点->右子树
	 * @param $node
	 */
	public function inorder($node)
	{
		if(is_null($node)){
			return;
		}
		$this->inorder($node->left);
		echo $node->val . '<br />';
		$this->inorder($node->right);
	}

	/**
	 * 后序遍历
	 * 左子树->右子树->根节点
	 * @param $node
	 */
	public function postorder($node)
	{
		if(is_null($node)){
			return;
		}
		$this->postorder($node->left);
		$this->postorder($node->right);
		echo $node->val . '<br />';
	}

}
```