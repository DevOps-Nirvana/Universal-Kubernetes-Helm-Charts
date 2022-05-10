TODO: Migrate this into Github Actions to run these, for now can do manually though...

# One time
helm repo add devopsnirvanas3 s3://devops-nirvana/helm-charts
helm repo add devops-nirvana https://devops-nirvana.s3.amazonaws.com/helm-charts/

helm lint charts/* --set name=test,namespace=test
# BUMP VERSION...
export CI_COMMIT_TAG=1.0.24
sed -i "s/1.0.0/$CI_COMMIT_TAG/g" charts/*/Chart.yaml
cd charts && for CURRENT_HELM_CHART in $(ls -d */ | tr '/' ' '); do helm package -u $CURRENT_HELM_CHART; done && cd ..
export AWS_DEFAULT_REGION=us-east-1
ls -la charts/*.tgz
for PUSH_HELM_CHART_TGZ in $(find ./charts -maxdepth 1 -name "*.tgz"); do helm s3 push --acl="public-read" $PUSH_HELM_CHART_TGZ devopsnirvanas3; done
# for PUSH_HELM_CHART_TGZ in $(find ./charts -maxdepth 1 -name "*.tgz"); do helm s3 push --acl="public-read" $PUSH_HELM_CHART_TGZ devopsnirvanas3 --force; done
# Grab the index.yaml file that is s3:// and make it https://
rm -f charts/*.tgz
rm -f index.yaml || true
wget https://devops-nirvana.s3.amazonaws.com/helm-charts/index.yaml
cat index.yaml | \
gsed "s/s3:/https:/g" | \
gsed "s|devops-nirvana/helm-charts|devops-nirvana.s3.amazonaws.com/helm-charts|g" > indexnew.yaml
diff index.yaml indexnew.yaml
mv -f indexnew.yaml index.yaml
aws s3 cp ./index.yaml s3://devops-nirvana/helm-charts/ --metadata-directive REPLACE --cache-control max-age=0,no-cache,no-store,must-revalidate --acl public-read --content-type 'text/plain'
rm -f index.yaml
git add farley-publish-notes.md
git commit -m "version bump to $CI_COMMIT_TAG"
# This next one undoes the version setting in the Chart.yamls
git reset --hard HEAD
# Save it upstream
git push


# https://devops-nirvana.s3.amazonaws.com/helm-charts/index.yaml
