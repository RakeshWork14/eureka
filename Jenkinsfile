// Kubernetes pipeline

@Library("i27academy-slb@main") _
    k8spipeline(
        appName: 'eureka'
    )






// Below is the docker pipeline, but we need to write the kubernetes pipeline

// // calling the shared library 
// // "i27academy-slb@main" this name is coming from
// // jenkins-> manage jenkins -> system -> Global Trusted Pipeline Libraries -> name(i27academy-slb) -> Defualt versin "you git branch" (main/master) -> git -> added shared library repo and save
// // "@main" you defualt version in manage jenkins or  git branch of your shared librarys
// @Library("i27academy-slb@main") _
//     dockerPipeline(
//         appName: 'eureka',
//         devPort: '5761',
//         testPort: '6761',
//         stagePort: '7761',
//         prodPort: '8761',
//         contPort: '8761'
//     )