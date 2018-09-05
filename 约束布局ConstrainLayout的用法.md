### 											一、约束布局ConstrainLayout的用法

1、ContraintLayout的优势：

- 较高的性能优势：布局层次越高，性能开销越大，而使用ConstraintLayout经常就一层嵌套就搞定，所以其性能要好很多。

- 完美的屏幕适配：ConstraintLayout的大小、距离都可以使用比例设置，所以其适配性更好。

- 书写简单

- 可视化編輯：可以通过代码或者xml进行編輯

  

2、ConstraintLayout的基本使用：

- 定位位置：提供了13个属性，哪一条边和哪一条边对齐。layout_constraint*属性的值可以是某个id或者parent
- B要位于A的右边，则使用app:layout_constraintLeft_toRightOf= "@+id/a",C位于A的下边，则使用app:layout_constraintTop_toBottomOf="@+id/A"
  - layout_constraintLeft_toLeftOf
  - layout_constraintLeft_toRightOf
  - layout_constraintRight_toLeftOf
  - layout_constraintRight_toRightOf
  - layout_constraintTop_toTopOf
  - layout_constraintTop_toBottomOf
  - layout_constraintBottom_toTopOf
  - layout_constraintBottom_toBottomOf
  - layout_constraintBaseline_toBaselineOf
  - layout_constraintStart_toEndOf
  - layout_constraintStart_toStartOf
  - layout_constraintEnd_toStartOf
  - layout_constraintEnd_toEndOf
- 设置margin还是继续用以前的属性layout_margin* 。 注意需要使magin生效，必须具有对应方向的layout_constraint*,否则magin不生效。