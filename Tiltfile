SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='harbor.dragonstone.tkg-aws-e2-lab.winterfell.be/tap-wkld/rob-java-web-app-local-source')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='dev-space')
allow_k8s_contexts('do-sfo3-k8s-tap')

k8s_custom_deploy(
    'rob-java-web-app',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --type web --label apps.tanzu.vmware.com/has-tests=true " +
               " --yes >/dev/null" +
               " && kubectl get workload rob-java-web-app --namespace " + NAMESPACE + " -o yaml",
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource('rob-java-web-app', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'serving.knative.dev/service': 'rob-java-web-app'}])
