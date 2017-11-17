---
layout:     post
comments:   true
title:      "bullet柔体模拟入门1"
date:       2017-11-16
author:     "ysd"
header-img: "img/post-bg-2015.jpg"
tags:
    - 物理引擎
    - bullet
---

#### 从网格创建柔体

{% highlight c++ %}
btSoftBody* btSoftBodyHelpers::CreateFromTriMesh(btSoftBodyWorldInfo& worldInfo,
const btScalar*	vertices, const int* triangles, int ntriangles, bool randomizeConstraints)
{
	int maxidx=0;
	int i,j,ni;

	for(i=0,ni=ntriangles*3;i<ni;++i)
	{
		maxidx=btMax(triangles[i],maxidx);
	}
	++maxidx;
	btAlignedObjectArray<bool> chks;
	btAlignedObjectArray<btVector3> vtx;
	chks.resize(maxidx*maxidx,false);
	vtx.resize(maxidx);
	for(i=0,j=0,ni=maxidx*3;i<ni;++j,i+=3)
	{
		vtx[j]=btVector3(vertices[i],vertices[i+1],vertices[i+2]);
	}
	// btSoftBody的构造函数里会创建Node，并加入到dbvt里，在精细阶段会用到dbvt，检测每个点和其他碰撞体

	btSoftBody*	psb=new btSoftBody(&worldInfo,vtx.size(),&vtx[0],0);
	for( i=0,ni=ntriangles*3;i<ni;i+=3)
	{
		const int idx[]={triangles[i],triangles[i+1],triangles[i+2]};
#define IDX(_x_,_y_) ((_y_)*maxidx+(_x_))

		// 创建Link链接三角形的三个顶点，只初始化了弹簧的初始长度
		
		for(int j=2,k=0;k<3;j=k++)
		{
			if(!chks[IDX(idx[j],idx[k])])
			{
				chks[IDX(idx[j],idx[k])]=true;
				chks[IDX(idx[k],idx[j])]=true;
				psb->appendLink(idx[j],idx[k]);
			}
		}
#undef IDX

		// 为每个三角形创建Face，用于柔体之间的碰撞检测，初始化了三角形的面积

		psb->appendFace(idx[0],idx[1],idx[2]);
	}

	if (randomizeConstraints)
	{
		psb->randomizeConstraints();
	}

	return(psb);
}
{% endhighlight %}